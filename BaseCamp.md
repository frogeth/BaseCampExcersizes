
https://sepolia-bridge.base.org/deposit

https://docs.base.org/base-camp/progress/

Deploying to a Testnet

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

  

contract BasicMath {

// Function to add two uints with overflow check

function adder(uint _a, uint _b) public pure returns (uint sum, bool error) {

// SafeMath library is not needed in Solidity 0.8.0 and above as overflow checks are built-in

unchecked {

sum = _a + _b;

}

if(sum < _a || sum < _b) {

// Overflow has occurred

return (0, true);

}

return (sum, false);

}

// Function to subtract two uints with underflow check

function subtractor(uint _a, uint _b) public pure returns (uint difference, bool error) {

if(_b > _a) {

// Underflow has occurred

return (0, true);

}

return (_a - _b, false);

}

}
```

Controlled Structures
```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

  

contract ControlStructures {

  

// Custom error for doNotDisturb function

error AfterHours(uint time);

  

// Function fizzBuzz as described, adhering to traditional rules

function fizzBuzz(uint _number) public pure returns (string memory) {

if (_number % 3 == 0 && _number % 5 == 0) {

return "FizzBuzz";

} else if (_number % 3 == 0) {

return "Fizz";

} else if (_number % 5 == 0) {

return "Buzz";

} else {

return "Splat";

}

}

  

// Function doNotDisturb as described

function doNotDisturb(uint _time) public pure returns (string memory) {

if (_time >= 2400) {

// Use assert to enforce that time must not be >= 2400, which will cause a panic if failed

assert(_time < 2400);

} else if (_time > 2200 || _time < 800) {

revert AfterHours(_time);

} else if (_time >= 1200 && _time <= 1259) {

revert("At lunch!");

} else if (_time >= 800 && _time <= 1199) {

return "Morning!";

} else if (_time >= 1300 && _time <= 1799) {

return "Afternoon!";

} else if (_time >= 1800 && _time <= 2200) {

return "Evening!";

}

// Given the conditions, this line should never be reached.

// Added to satisfy function's return requirement.

return "Undefined time";

}

}
```

EmployeeStorage

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

  

contract EmployeeStorage {

uint256 public idNumber;

string public name;

uint32 private salary;

uint16 private shares;

  

constructor() {

shares = 1000;

name = "Pat";

salary = 50000;

idNumber = 112358132134;

}

  

function viewSalary() public view returns (uint32) {

return salary;

}

  

function viewShares() public view returns (uint16) {

return shares;

}

  

function grantShares(uint16 _newShares) public {

if (_newShares + shares > 5000) {

revert("Too many shares");

}

shares += _newShares;

}

  

function checkForPacking(uint _slot) public view returns (uint r) {

assembly {

r := sload(_slot)

}

}

  

function debugResetShares() public {

shares = 1000;

}

}
```

ArraysExcersize

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

  

contract ArraysExercise {

uint[] public numbers = [1,2,3,4,5,6,7,8,9,10];

address[] public senders;

uint[] public timestamps;

  

function getNumbers() public view returns (uint[] memory) {

return numbers;

}

  

function resetNumbers() public {

delete numbers;

numbers = [1,2,3,4,5,6,7,8,9,10];

}

  

function appendToNumbers(uint[] calldata _toAppend) public {

for (uint i = 0; i < _toAppend.length; i++) {

numbers.push(_toAppend[i]);

}

}

  

function saveTimestamp(uint _unixTimestamp) public {

senders.push(msg.sender);

timestamps.push(_unixTimestamp);

}

  

function afterY2K() public view returns (uint[] memory, address[] memory) {

uint[] memory validTimestamps = new uint[](timestamps.length);

address[] memory validSenders = new address[](senders.length);

uint count = 0;

for (uint i = 0; i < timestamps.length; i++) {

if (timestamps[i] > 946702800) {

validTimestamps[count] = timestamps[i];

validSenders[count] = senders[i];

count++;

}

}

  

// Adjust array size to fit count

uint[] memory filteredTimestamps = new uint[](count);

address[] memory filteredSenders = new address[](count);

for (uint i = 0; i < count; i++) {

filteredTimestamps[i] = validTimestamps[i];

filteredSenders[i] = validSenders[i];

}

  

return (filteredTimestamps, filteredSenders);

}

  

function resetSenders() public {

delete senders;

}

  

function resetTimestamps() public {

delete timestamps;

}

}
```

Mappings

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

  

contract FavoriteRecords {

mapping(string => bool) public approvedRecords;

mapping(address => mapping(string => bool)) public userFavorites;

string[] private approvedRecordNames = [

"Thriller",

"Back in Black",

"The Bodyguard",

"The Dark Side of the Moon",

"Their Greatest Hits (1971-1975)",

"Hotel California",

"Come On Over",

"Rumours",

"Saturday Night Fever"

];

constructor() {

for(uint i = 0; i < approvedRecordNames.length; i++) {

approvedRecords[approvedRecordNames[i]] = true;

}

}

  

error NotApproved(string albumName);

  

function addRecord(string memory albumName) public {

if (!approvedRecords[albumName]) {

revert NotApproved(albumName);

}

userFavorites[msg.sender][albumName] = true;

}

  

function getApprovedRecords() public view returns (string[] memory) {

return approvedRecordNames;

}

  

function getUserFavorites(address user) public view returns (string[] memory) {

uint256 count = 0;

for(uint i = 0; i < approvedRecordNames.length; i++) {

if (userFavorites[user][approvedRecordNames[i]]) {

count++;

}

}

string[] memory favorites = new string[](count);

uint256 index = 0;

for(uint i = 0; i < approvedRecordNames.length; i++) {

if (userFavorites[user][approvedRecordNames[i]]) {

favorites[index++] = approvedRecordNames[i];

}

}

return favorites;

}

  

function resetUserFavorites() public {

for(uint i = 0; i < approvedRecordNames.length; i++) {

delete userFavorites[msg.sender][approvedRecordNames[i]];

}

}

}
```

Structs

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

  

contract GarageManager {

struct Car {

string make;

string model;

string color;

uint numberOfDoors;

}

  

mapping(address => Car[]) public garage;

  

error BadCarIndex(uint index);

  

function addCar(string memory make, string memory model, string memory color, uint numberOfDoors) public {

Car memory newCar = Car(make, model, color, numberOfDoors);

garage[msg.sender].push(newCar);

}

  

function getMyCars() public view returns (Car[] memory) {

return garage[msg.sender];

}

  

function getUserCars(address user) public view returns (Car[] memory) {

return garage[user];

}

  

function updateCar(uint index, string memory make, string memory model, string memory color, uint numberOfDoors) public {

if (index >= garage[msg.sender].length) {

revert BadCarIndex(index);

}

garage[msg.sender][index] = Car(make, model, color, numberOfDoors);

}

  

function resetMyGarage() public {

delete garage[msg.sender];

}

}
```

Inheritence

This ones more complicated: 
### Step 1: Deploy `Salesperson` and `EngineeringManager`

- Go to the "Deploy & Run Transactions" plugin.
- Select `Salesperson` from the contract dropdown.
    - Fill in the `Salesperson` constructor parameters: `55555` for `_idNumber`, `12345` for `_managerId`, and `20` for `_hourlyRate`.
    - Click "Deploy".
- Select `EngineeringManager` from the contract dropdown.
    - Fill in the `EngineeringManager` constructor parameters: `54321` for `_idNumber`, `11111` for `_managerId`, and `200000` for `_annualSalary`.
    - Click "Deploy".

### Step 2: Deploy `InheritanceSubmission`

- After deploying both `Salesperson` and `EngineeringManager`, copy their deployed addresses from the "Deployed Contracts" section in Remix.
- Select `InheritanceSubmission` from the contract dropdown.
    - Fill in the constructor parameters with the addresses of your deployed `Salesperson` and `EngineeringManager` contracts.
    - Click "Deploy".

### Step 3: Submit `InheritanceSubmission`

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

  

abstract contract Employee {

uint public idNumber;

uint public managerId;

  

constructor(uint _idNumber, uint _managerId) {

idNumber = _idNumber;

managerId = _managerId;

}

  

function getAnnualCost() public virtual view returns (uint);

}

  

contract Salaried is Employee {

uint public annualSalary;

  

constructor(uint _idNumber, uint _managerId, uint _annualSalary) Employee(_idNumber, _managerId) {

annualSalary = _annualSalary;

}

  

function getAnnualCost() public view override returns (uint) {

return annualSalary;

}

}

  

contract Hourly is Employee {

uint public hourlyRate;

  

constructor(uint _idNumber, uint _managerId, uint _hourlyRate) Employee(_idNumber, _managerId) {

hourlyRate = _hourlyRate;

}

  

function getAnnualCost() public view override returns (uint) {

return hourlyRate * 2080;

}

}

  

contract Manager {

uint[] public employeeIds;

  

function addReport(uint _employeeId) public {

employeeIds.push(_employeeId);

}

  

function resetReports() public {

delete employeeIds;

}

}

  

contract Salesperson is Hourly {

constructor(uint _idNumber, uint _managerId, uint _hourlyRate) Hourly(_idNumber, _managerId, _hourlyRate) {}

}

  

contract EngineeringManager is Salaried, Manager {

constructor(uint _idNumber, uint _managerId, uint _annualSalary) Salaried(_idNumber, _managerId, _annualSalary) {}

}

  

contract InheritanceSubmission {

address public salesPerson;

address public engineeringManager;

  

constructor(address _salesPerson, address _engineeringManager) {

salesPerson = _salesPerson;

engineeringManager = _engineeringManager;

}

}
```

Imports

Create a folder then create two files (EXACT NAMING IS IMPORTANT): 

SillyStringUtils.sol

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.17;

  

library SillyStringUtils {

struct Haiku {

string line1;

string line2;

string line3;

}

  

function shruggie(string memory input) internal pure returns (string memory) {

return string(abi.encodePacked(input, unicode" 🤷"));

}

}
```

ImportsExcersize.sol

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.23;

  

import "./SillyStringUtils.sol";

  

contract ImportsExercise {

using SillyStringUtils for *;

SillyStringUtils.Haiku private haiku;

  

function saveHaiku(string calldata line1, string calldata line2, string calldata line3) external {

haiku.line1 = line1;

haiku.line2 = line2;

haiku.line3 = line3;

}

  

function getHaiku() external view returns (SillyStringUtils.Haiku memory) {

return haiku;

}

  

function shruggieHaiku() external view returns (SillyStringUtils.Haiku memory) {

SillyStringUtils.Haiku memory modifiedHaiku = SillyStringUtils.Haiku({

line1: haiku.line1,

line2: haiku.line2,

line3: SillyStringUtils.shruggie(haiku.line3)

});

  

return modifiedHaiku;

}

}
```

Create a base scan account: https://basescan.org/ 
Hover over username, go to API KEYS 
Create an api key
Then go to Remix at the bottom left corner there is an Plugin Manger option above the settings cog wheel
Search for CONTRACT VERIFICATION - ETHERSCAN and activate it
Click the etherscan icon on the left sidebar and input your api key
Then compile "ImportsExcersize.sol" and deploy it
Copy the contract address over to the contract verification plugin and make sure "ImportsExcersize.sol" is selected
Now you can submit

The "new" keyword

Create a folder
Make 2 contracts:

"AddressBook.sol"
```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.23;

  

import "@openzeppelin/contracts/access/Ownable.sol";

import "@openzeppelin/contracts/utils/Counters.sol";

  

contract AddressBook is Ownable {

using Counters for Counters.Counter;

Counters.Counter private _contactIds;

  

struct Contact {

uint id;

string firstName;

string lastName;

uint[] phoneNumbers;

}

  

mapping(uint => Contact) private _contacts;

  

// Adjusted constructor to accept the initial owner address

constructor(address initialOwner) Ownable(initialOwner) {}

  

function addContact(string memory firstName, string memory lastName, uint[] memory phoneNumbers) public onlyOwner {

uint contactId = _contactIds.current();

_contactIds.increment();

_contacts[contactId] = Contact(contactId, firstName, lastName, phoneNumbers);

}

  

function deleteContact(uint contactId) public onlyOwner {

require(_contacts[contactId].id != 0, "ContactNotFound");

delete _contacts[contactId];

}

  

function getContact(uint contactId) public view returns (Contact memory) {

require(_contacts[contactId].id != 0, "ContactNotFound");

return _contacts[contactId];

}

  

function getAllContacts() public view returns (Contact[] memory) {

uint total = _contactIds.current();

uint actualCount = 0;

for(uint i = 0; i < total; i++) {

if(_contacts[i].id != 0) {

actualCount++;

}

}

  

Contact[] memory contacts = new Contact[](actualCount);

uint counter = 0;

for(uint i = 0; i < total; i++) {

if(_contacts[i].id != 0) {

contacts[counter] = _contacts[i];

counter++;

}

}

return contacts;

}

}
```

"AddressBookFactory.sol"

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.23;

import "./AddressBook.sol";

  

contract AddressBookFactory {

event AddressBookCreated(address indexed addressBookAddress, address indexed owner);

  

function deploy() public returns (address) {

AddressBook newAddressBook = new AddressBook(msg.sender);

emit AddressBookCreated(address(newAddressBook), msg.sender);

return address(newAddressBook);

}

}
```

Then deploy AddressBookFactory.sol

Minimal Tokens

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract UnburnableToken {
    mapping(address => uint256) public balances;
    uint256 public totalSupply = 100000000;
    uint256 public totalClaimed;

    error TokensClaimed();
    error AllTokensClaimed();
    error UnsafeTransfer(address to);

    function claim() public {
        if (totalClaimed + 1000 > totalSupply) revert AllTokensClaimed();
        if (balances[msg.sender] > 0) revert TokensClaimed();

        balances[msg.sender] += 1000;
        totalClaimed += 1000;
    }

    function safeTransfer(address _to, uint256 _amount) public {
        if (_to == address(0) || !hasGoerliETH(_to)) revert UnsafeTransfer(_to);
        if (balances[msg.sender] < _amount) revert UnsafeTransfer(msg.sender);

        balances[msg.sender] -= _amount;
        balances[_to] += _amount;
    }

    function hasGoerliETH(address _address) public view returns (bool) {
        return _address.balance > 0;
    }
}

```

ERC20

```
// SPDX-License-Identifier: MIT

  

pragma solidity ^0.8.17;

  

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

import "@openzeppelin/contracts/utils/structs/EnumerableSet.sol";

  

error TokensClaimed();

error AllTokensClaimed();

error NoTokensHeld();

error QuorumTooHigh();

error AlreadyVoted();

error VotingClosed();

  

contract WeightedVoting is ERC20 {

using EnumerableSet for EnumerableSet.AddressSet;

uint256 public maxSupply;

uint256 public totalClaimed;

  

struct Issue {

EnumerableSet.AddressSet voters;

string issueDesc;

uint256 votesFor;

uint256 votesAgainst;

uint256 votesAbstain;

uint256 totalVotes;

uint256 quorum;

bool isPassed;

bool isClosed;

}

  

struct ReturnableIssue {

address[] voters;

string issueDesc;

uint256 votesFor;

uint256 votesAgainst;

uint256 votesAbstain;

uint256 totalVotes;

uint256 quorum;

bool isPassed;

bool isClosed;

}

  

Issue[] issues;

  

enum Votes { AGAINST, FOR, ABSTAIN }

  

constructor(string memory _name, string memory _symbol) ERC20(_name, _symbol) {

maxSupply = 1000000;

issues.push();

}

  

function claim() external {

if (totalClaimed == maxSupply) {

revert AllTokensClaimed();

}

if (balanceOf(msg.sender) >= 100) {

revert TokensClaimed();

}

  

_mint(msg.sender, 100);

totalClaimed += 100;

}

  

function createIssue(string memory _issueDesc, uint256 _quorum) external returns (uint256) {

if (balanceOf(msg.sender) == 0) {

revert NoTokensHeld();

}

if (_quorum > totalClaimed) {

revert QuorumTooHigh();

}

  

uint256 newIssueIndex = issues.length;

issues.push();

Issue storage newIssue = issues[newIssueIndex];

newIssue.issueDesc = _issueDesc;

newIssue.quorum = _quorum;

  

return newIssueIndex;

}

  

function getIssue(uint256 _issueId) external view returns (ReturnableIssue memory) {

Issue storage issue = issues[_issueId];

  

address[] memory issueVoters = new address[](issue.voters.length());

for (uint256 i = 0; i < issue.voters.length(); i++){

issueVoters[i] = issue.voters.at(i);

}

  

ReturnableIssue memory result;

result.voters = issueVoters;

result.issueDesc = issue.issueDesc;

result.votesFor = issue.votesFor;

result.votesAgainst = issue.votesAgainst;

result.votesAbstain = issue.votesAbstain;

result.totalVotes = issue.totalVotes;

result.quorum = issue.quorum;

result.isPassed = issue.isPassed;

result.isClosed = issue.isClosed;

  

return result;

}

  

function vote(uint256 _issueId, Votes _vote) external {

Issue storage issue = issues[_issueId];

  

if(issue.isClosed) {

revert VotingClosed();

}

if(issue.voters.contains(msg.sender)) {

revert AlreadyVoted();

}

  

uint256 voterBalance = balanceOf(msg.sender);

  

if (_vote == Votes.FOR) {

issue.votesFor += voterBalance;

} else if (_vote == Votes.AGAINST) {

issue.votesAgainst += voterBalance;

} else if (_vote == Votes.ABSTAIN) {

issue.votesAbstain += voterBalance;

}

  

issue.totalVotes += voterBalance;

  

issue.voters.add(msg.sender);

  

if (issue.totalVotes >= issue.quorum) {

issue.isClosed = true;

  

if (issue.votesFor > issue.votesAgainst) {

issue.isPassed = true;

}

if (issue.quorum > totalSupply()) revert QuorumTooHigh();

if (balanceOf(msg.sender) == 0) revert NoTokensHeld();

}

}

}
```

ERC721

```
// SPDX-License-Identifier: MIT

  

pragma solidity ^0.8.17;

  

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC721/ERC721.sol";

  

error HaikuNotUnique();

error NotYourHaiku();

error NoHaikusShared();

  

contract HaikuNFT is ERC721 {

struct Haiku {

address author;

string line1;

string line2;

string line3;

}

  

Haiku[] public haikus;

mapping(address => uint256[]) public sharedHaikus;

uint256 public counter;

  

constructor(string memory _name, string memory _symbol) ERC721(_name, _symbol) {

counter = 1;

}

  

function mintHaiku(string memory _line1, string memory _line2, string memory _line3) external {

for (uint256 i = 0; i < haikus.length; i++) {

if (keccak256(abi.encodePacked(haikus[i].line1)) == keccak256(abi.encodePacked(_line1)) ||

keccak256(abi.encodePacked(haikus[i].line2)) == keccak256(abi.encodePacked(_line2)) ||

keccak256(abi.encodePacked(haikus[i].line3)) == keccak256(abi.encodePacked(_line3))) {

revert HaikuNotUnique();

}

}

haikus.push(Haiku(msg.sender, _line1, _line2, _line3));

_safeMint(msg.sender, counter);

counter++;

}

  

function shareHaiku(address _to, uint256 _haikuId) external {

require(ownerOf(_haikuId) == msg.sender, "NotYourHaiku");

sharedHaikus[_to].push(_haikuId);

}

  

function getMySharedHaikus() external view returns (uint256[] memory) {

uint256[] memory result = sharedHaikus[msg.sender];

if (result.length == 0) {

revert NoHaikusShared();

}

return result;

}

}
```