# Petting Aavegotchis

## GotchiCare

[GotchiCare](https://gotchicare.com) uses Gelato Ops to provide an automated Aavegotchi petting service.

Gelato calls `exec` here which pets the Aavegotchis and settles the fee.&#x20;

```solidity
  function exec(CareInfo calldata _careInfo) external {
    require(msg.sender == executor, "Carer: Only executor");

    bytes32 _receipt = getReceipt(_careInfo.owner, _careInfo);

    require(
      caringOwners.contains(_careInfo.owner),
      "Carer: exec: Owner has not started"
    );
    require(
      _careInfo.owner == ownerOfReceipt[_receipt],
      "Carer: exec: Receipt does not match"
    );

    operator.pet(_careInfo.gotchis, _careInfo.owner);

    payWages(_careInfo.owner, _careInfo.rate);

 
    CareInfo memory newCareInfo = CareInfo(
      _careInfo.owner,
      _careInfo.pets.add(1),
      _careInfo.rate,
      _careInfo.gotchis
    );

      updateOwnerInfo(newCareInfo);

  }
```

This resolver loops through an array of subscribed users. For each user, it checks if their aavegotchi's petting time has reached.&#x20;

```solidity
  function checker()
    external
    view
    returns (bool canExec, bytes memory execPayload)
  {
    address[] memory _caringOwners = careCentre.getCaringOwners();
    uint256 _length = _caringOwners.length;

    for (uint256 i = 0; i < _length; i++) {
      ICareCentre.CareInfo memory _careInfo = careCentre.getCareInfoByOwner(
        _caringOwners[i]
      );

      if (!ownerHasBalance(_caringOwners[i], _careInfo.rate)) continue;
      if (!isApproved(_caringOwners[i])) continue;

      uint256[] memory _gotchis = _careInfo.gotchis;

      uint256 _lastInteracted = gotchiFacet
        .getAavegotchi(_gotchis[0])
        .lastInteracted;

      uint256 _nextInteract = _lastInteracted + 12 hours;

      if (block.timestamp >= _nextInteract) {
        canExec = true;

        execPayload = abi.encodeWithSelector(
          ICareCentre.exec.selector,
          _careInfo
        );

        return (canExec, execPayload);
      }
    }

    canExec = false;
  }
```

