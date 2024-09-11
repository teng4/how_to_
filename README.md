# How to create a customized ROS msg in a catkin_ws?

In ROS, custom message types are defined using `.msg` files, which are stored in a package's msg folder. These message types allow you to define custom data structures that can be used for inter-node communication. Below is a guide on how to create and use custom message types in a `catkin_ws` workspace.

Steps to Customize a msg Folder and Files in a `catkin_ws`.

## 1. Create a ROS Package

First, create a package in your `catkin_ws` workspace. This package will hold your custom messages.

```
cd ~/catkin_ws/src
catkin_create_pkg my_custom_msgs std_msgs rospy roscpp
```

Here, `my_custom_msgs` is the package name, and it depends on `std_msgs`, `rospy`, and `roscpp`.

## 2. Create a `msg` Directory

Inside your package, create a `msg` folder where your custom message types will be defined.

```
cd ~/catkin_ws/src/my_custom_msgs
mkdir msg
```

## 3. Create a '.msg' File

Inside the `msg` folder, create a file with a `.msg` extension. This file defines your custom message type.

Example: `Position.ms`

```
gedit msg/Position.msg
```

Inside `Position.msg`, define the structure of the message. For example, for a 3D position:

```
float64 x
float64 y
float64 z
```

Each line defines a field in the message, with the type followed by the field name.

## 4. Modify CMakeLists.txt

You need to modify your package's `CMakeLists.txt` file to include the message generation.

- Find and uncomment (or add) the following lines in the `CMakeLists.txt`:

```
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  message_generation
)
```

- Add the custom message files under `add_message_files`:

```
add_message_files(
  FILES
  Position.msg
)
```

- Ensure that `message_generation` is added to the `catkin_package()` macro:

```
catkin_package(
  CATKIN_DEPENDS message_runtime std_msgs
)
```

- Add the following after `add_message_files()` to actually generate the messages:

```
generate_messages(
  DEPENDENCIES
  std_msgs
)
```

## 5. Modify `package.xml`
In your package's `package.xml`, ensure you have the following dependencies for message generation:

```
<build_depend>message_generation</build_depend>
<exec_depend>message_runtime</exec_depend>
```

## 6. Build the Package
After modifying the files, go back to the root of your `catkin_ws` workspace and build the workspace.

```
cd ~/catkin_ws
catkin_make
```




























