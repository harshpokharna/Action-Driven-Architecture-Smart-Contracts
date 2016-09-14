# Action-Driven-Architecture-Smart-Contracts
*Demonstrating the Action Driven Architecture by Eris Industries*

Previously [here](https://github.com/harshpokharna/The-5-Types-Model-Simple-Bank-System-Solidity), I demonstrated the 5 types model for Smart Contracts. (I recommend readers to quickly go through the Readme.md for better understanding)

In short, this diagram explains the 5 types model architecture

![Alt text](https://github.com/harshpokharna/Action-Driven-Architecture-Smart-Contracts/blob/master/res/action_driven_arch.png "Architecture for 5 types model")

And we had a ContractNameRegistry Service which stored the addresses for all the contracts

![Alt text](https://github.com/harshpokharna/Action-Driven-Architecture-Smart-Contracts/blob/master/res/interacton_with_registry.png "Interaction with Registry")

But there are a few problems associated with this architecture:
- Let's say we have 10 controllers and 10 databases. Each controller has 4 functions. Then the ALC must have 40 functions to interact with controllers. In case, if we update or change a controller, we have to replace the ALC as well.

This problem can be eliminated by using an Action Based Architecture.

![Alt text](https://github.com/harshpokharna/Action-Driven-Architecture-Smart-Contracts/blob/master/res/action_driven_arch.png "Action Driven Architectue")

This system is deployed in the following way:
- First deploy the ContractNameRegistry
- Then deploy the ActionManager and ActionDB in any order and register them with the ContractNameRegistry
- Next deploy all the actions and register them with the ActionManager which in turn interacts with the ActionDB
- Finally deploy the database contracts and register them with the ContractNameRegistry

The flow thus works as follows:
- All the incoming transactions come to the ActionManager
- ActionManager then gets the address of ActionDB from the ContractNameRegistry
- ActionManager then gets the required action address from AcionDB
- ActionManager then calls the required Action
- Action gets the address for Database Contract from the ContractNameRegistry
- Action calls the Database Contract

Thus
- ActionManager can be called by anyone
- ContractNameRegistry can be called by anyone
- Only Actions can call Database Contracts

To have a more sophesticated permissioning model:
- There can be a UserPermissionDB which stores permissions for outside users to control access to ActionManager. Whenever there is a call from outside, ActionManager can verify if the user has required permissions.
- There can be a ActionPermissionDB which stores permissions for all the actions. Whenever a Database Contract gets a call, it can verify if that particular Action has permission to call it.
- 

