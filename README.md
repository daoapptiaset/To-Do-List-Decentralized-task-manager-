# To-Do-List-Decentralized-task-manager-
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TodoList {
    struct Task {
        uint256 id;
        string content;
        bool completed;
    }

    mapping(address => Task[]) public userTasks;
    uint256 public taskCount;

    event TaskCreated(address user, uint256 id, string content);
    event TaskToggled(address user, uint256 id, bool completed);

    function createTask(string memory _content) public {
        taskCount++;
        userTasks[msg.sender].push(Task(taskCount, _content, false));
        emit TaskCreated(msg.sender, taskCount, _content);
    }

    function toggleCompleted(uint256 _id) public {
        Task[] storage tasks = userTasks[msg.sender];
        for (uint i = 0; i < tasks.length; i++) {
            if (tasks[i].id == _id) {
                tasks[i].completed = !tasks[i].completed;
                emit TaskToggled(msg.sender, _id, tasks[i].completed);
                return;
            }
        }
    }
}
