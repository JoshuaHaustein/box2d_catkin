# box2d_catkin

This package is essentially just a wrapper around Box2D. 
It contains a CMakeLists and package.xml so that Box2D can be built
with catkin_make.

This repository contains Box2D as a submodule - that is as a repository within the repository.
Therefore, when cloning this repository you either need to clone it using the command:

> git clone --recursive https://github.com/JoshuaHaustein/box2d_catkin.git

Or, if you already cloned the repository, cd to it and run the commands:

> git submodule init

> git submodule update

In any case, once Box2D is checked out, cd into the subfolder box2d_catkin/Box2D and checkout version
2.3.1:

> git checkout v2.3.1

Once you did this, you should be able to build Box2D using catkin_make.
