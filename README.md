# solidity-project-1
solidity project of eth lottery 
//SPDX-License-Identifier: GPL-3.0

pragma solidity >= 0.5.0 <0.9.0;

contract Lottery
{
    address public organisation;
    address payable[] public participants;

    constructor()
    {
            organisation=msg.sender; //global variable
    }

    receive() external payable
    {
        require(msg.value==0.05 ether);
            participants.push(payable(msg.sender));
    }

    function getBalance() public view returns(uint)
    {
        require(msg.sender== organisation);
        return address(this).balance;
    }

    function random() public view returns(uint)
    {
             return uint(keccak256(abi.encodePacked(block.difficulty, block.timestamp, participants.length)));
    }

    function selectWinner() public
    {
        require(msg.sender==organisation);
        require(participants.length>=3);
        address payable winner;
        uint r=random();
        uint index = r % participants.length;
        winner = participants[index];
        winner.transfer(getBalance());
        participants=new address payable[](0);
    }
}
