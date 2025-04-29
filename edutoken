// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

/**
 * @title EduToken
 * @dev ERC20 token for educational rewards system that allows educational institutions 
 * to reward students for academic achievements
 */
contract EduToken is ERC20, Ownable {
    // Mapping to track registered educational institutions
    mapping(address => bool) public registeredInstitutions;
    
    // Mapping to track student achievements
    mapping(address => uint256) public studentAchievements;
    
    // Events
    event InstitutionRegistered(address indexed institution);
    event InstitutionRemoved(address indexed institution);
    event AchievementRewarded(address indexed student, uint256 amount, string achievementType);
    
    /**
     * @dev Constructor initializes the EduToken with initial supply to contract owner
     * @param initialSupply The initial token supply (in the smallest unit)
     */
    constructor(uint256 initialSupply) ERC20("EduToken", "EDT") Ownable(msg.sender) {
        _mint(msg.sender, initialSupply);
    }
    
    /**
     * @dev Register or remove an educational institution's ability to distribute rewards
     * @param institution Address of the educational institution
     * @param isRegistered Boolean indicating if institution should be registered (true) or removed (false)
     */
    function manageInstitution(address institution, bool isRegistered) external onlyOwner {
        require(institution != address(0), "Invalid institution address");
        
        registeredInstitutions[institution] = isRegistered;
        
        if (isRegistered) {
            emit InstitutionRegistered(institution);
        } else {
            emit InstitutionRemoved(institution);
        }
    }
    
    /**
     * @dev Allows registered institutions to reward students for achievements
     * @param student Address of the student being rewarded
     * @param amount Amount of tokens to reward
     * @param achievementType String describing the type of achievement
     */
    function rewardAchievement(address student, uint256 amount, string calldata achievementType) external {
        require(registeredInstitutions[msg.sender], "Only registered institutions can reward achievements");
        require(student != address(0), "Invalid student address");
        require(amount > 0, "Reward amount must be greater than zero");
        
        // Transfer tokens from the institution to the student
        _transfer(msg.sender, student, amount);
        
        // Record the achievement
        studentAchievements[student] += amount;
        
        emit AchievementRewarded(student, amount, achievementType);
    }
}
