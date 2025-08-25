*21-08-2025 12:24*
### Status: 
  
### Tags: [[General knowledge]],


# Kernel architectures

An operating system allows the user application programs to interact with the system hardware. Since the operating system is such a complex structure, its architecture plays an important role in its usage. Each component of the Operating System Architecture should be well defined with clear inputs, outputs and functions.

## Import Terms

In operating system Architecture, we've two major terms which defines the major components of the operating systems.

- **Kernal** − Kernal is the central component of an operating system architecture in most of the implementation. A kernal is responsible for all major operations and interaction with the hardware. A kernal manages memory, processor, input/output devices and provides interface to application programs to interact with hardware components.

- **Shell** − Shell is an interface of an operating system. It can be command line interface or a graphical user interface. User interacts with an operating system using shell. Application programs can also use shell interface to interact with underlying operating system.

- **System Softwares** − System softwares are the programs which interact with Kernal and provides interface for security managment, memory management and other low level activities.

- **Application Programs** − Application softwares/Programs are the one using which a user interacts with the operating system. For example a word processor to create a document and save it on the file system, a notepad to create notes etc.
---
## Popular Architectures

Following are various popular implementations of Operating System architectures.

- Simple Architecture

- Monolith Architecture

- Micro-Kernel Architecture

- Exo-Kernel Architecture

- Layered Architecture    

- Modular Architecture

- Virtual Machine Architecture

- Hybrid architecture (windows nt kernel)
## Simple Architectures

##### Advantages

There are many operating systems that have a rather simple structure. These started as small systems and rapidly expanded much further than their scope. A common example of this is MS-DOS. It was designed simply for a niche amount for people

Following are advantages of a simple operating system architecture.

- **Easy Development** - In simple operation system, being very few interfaces, development is easy especially when only limited functionalities are to be delivered.
    
- **Better Performance** - Such a sytem, as have few layers and directly interects with hardware, can provide a better performance as compared to other types of operating systems.
    

##### Disadvantages

Following are disadvantages of a simple operating system architecture.

- **Frequent System Failures** - Being poorly designed, such a system is not robust. If one program fails, entires operating system crashses. Thus system failures are quiet frequent in simple operating systems.
    
- **Poor Maintainability** - As all layers of operating systems are tightly coupled, change in one layer can impact other layers heavily and making code unmanageable over a period of time.

## Monolith Architecture

In monolith architecture operating system, a central piece of code called kernel is responsible for all major operations of an operating system. Such operations includes file management, memory management, device management and so on. The kernal is the main component of an operating system and it provides all the services of an operating system to the application programs and system programs.

The kernel has access to the all the resources and it acts as an interface with application programs and the underlying hardware. A monolithic kernel architecture promotes timesharing, multiprogramming model and was used in old banking systems and [[GNU Linux]] .

![[Pasted image 20250821124705.png]]
##### Advantages

Following are advantages of a monolith operating system architecture.

- **Easy Development** - As kernel is the only layer to develop with all major functionalities, it is easier to design and develop.

- **Performance** - As Kernel is responsible for memory management, other operations and have direct access to the hardware, it performs better.


##### Disadvantages

Following are disadvantages of a monolith operating system architecture.

- **Crash Prone** - As Kernel is responsible for all functions, if one function fails entire operating system fails.

- **Difficult to enhance** - It is very difficult to add a new service without impacting other services of a monolith operating system.

## Micro-Kernel Architecture

As in case monolith architecture, there was single kernel, in micro-kernel, we have multiple kernels each one specilized in particular service. Each microkernel is developed independent to the other one and makes system more stable. If one kernel fails the operating sytem will keep working with other kernel's functionalities.

![Operating System Micro-Kernel Architecture](https://www.tutorialspoint.com/operating_system/images/os_microkernel_structure.jpg)

#### Advantages

Following are advantages of a microkernel operating system architecture.

- **Reliable and Stable** - As multiple kernels are working simultaneously, chances of failure of operating sytem is very less. If one functionlity is down, operating system can still provide other functionalities using stable kernels.

- **Maintainability** - Being small sized kernels, code size is maintainable. One can enhance a microkernel code base without impacting other microkernel code base.


#### Disadvantages

Following are disadvantages of a microkernel operating system architecture.

- **Complex to Design** - Such a microkernel based architecture is difficult to design.

- **Performance Degradation** - Multi kernel, Multi-modular communication may hamper the performance as compared to monolith architecture.

# References:

- [[Linux Kernel(drivers , Modules)]]
- 