# Token-Vesting
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TokenVesting {
    address public beneficiary;
    uint256 public start;
    uint256 public duration;
    uint256 public released;

    constructor(address _beneficiary, uint256 _duration) {
        beneficiary = _beneficiary;
        start = block.timestamp;
        duration = _duration;
    }

    function release(uint256 amount) public {
        require(msg.sender == beneficiary, "Only beneficiary");
        uint256 vested = (block.timestamp - start) * amount / duration;
        uint256 toRelease = vested - released;
        require(toRelease >= amount, "Not enough vested");
        released += amount;
        payable(beneficiary).transfer(amount); // giả sử là ETH vesting
    }
}
