# EE599 Final Project - TrojanMap

## TrojanMap

![](img/TrojanMap.png)
This project focuses on using data structures and graph search algorithms to build a mapping application.

- Please clone the repository, look through [README.md](README.md) and fill up functions to finish in the project.
- Please make sure that your code can run `bazel run/test`.
- In this project, you will need to fill up trojanmap.cc and add unit tests in tests in tests.
- Do **Not** change or modify any given function headers and formats in both trojanmap.cc. Unexpected changes will result in zero credit.
- For coding questions, there is a **black box** testing for each question. All points are only based on passing the test cases or not (i.e. we don't grade your work by your source code). So, try to do comprehensive testing before your final submission.
- For submission, please push your answers to Github before the deadline.
- Deadline:
  - **Video presentation**: Monday November 23rd (In the class). Each team should create a 1 to 2 minute presentation that includes: quick introduction of the team, explanation of the solution architecture (High level. Use slides and some graphs. No need to go into code details. Focus on one interesting part and explain that if you want.)
  - **Final report: Friday**, November 27th by 6:30 pm
- Total: 120 points. 100 points is considered full credit.

---

## The data Structure

Each point on the map is represented by the class **Node** showns below and defined in [trojanmap.h](src/lib/trojanmap.h).

```cpp
class Node {
  std::string id; // A unique id assign to each point
  double lat;     // Latitude
  double lon;     // Longitude
  std::string name; // Name of the location. E.g. "Bank of America".
  std::vector<std::string> neighbors; // List of the ids of all neighbor points.
};
```

---

## Prerequisites:

### OpenCV Installation

For visualizations we use OpenCV library. You will use this library as blackbox and don't need to worry about the graphic details.

**Notice**: Installing this library may take a long time. This is only for visualization. You can still start coding even without installing this library and visualization.

Use the following commands to install OpenCV.

```shell
cd TrojanMap
git clone https://github.com/opencv/opencv.git
cd opencv/
mkdir build install
cd build
```

Next, type the following, but make sure that you set the **path_to_install_folder** to be the absolute path to the install folder under opencv.

```shell
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=path_to_install_folder ..
make install
```

For example, if cloned this repo under "/Users/ari/github/TrojanMap", you should type:

```shell
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/Users/ari/github/TrojanMap/opencv/install ..
make install
```

---

## Run the program

Please run:

```shell
bazel run src/main:main
```

If everything is correct, this menu will show up.

```shell
**************************************************************
* Select the function you want to execute.
* 1. Autocomplete
* 2. Find the position
* 3. CalculateShortestPath
* 4. Traveling salesman problem
* 5. Exit
**************************************************************
```

## Your task is to implement a function for each menu item.

## Step 1: Autocomplete the location name

**HONG Shuo TODO**: Please double check the function declarations here match the code.

```c++
std::vector<std::string> Autocomplete(std::string input);
```

We consider the names of nodes as the locations. Implement a method to type the partial name of the location and return a list of possible locations with partial name as prefix. Please treat uppercase and lower case as the same character.

Example:

Input: "ch" \
Output: ["ChickfilA", "Chipotle Mexican Grill"]

Input: "ta" \
Output: ["Target", "Tap Two Blue"]

```shell
1
**************************************************************
* 1. Autocomplete
**************************************************************

Please input a partial location:ch
*************************Results******************************
ChickfilA
Chipotle Mexican Grill
**************************************************************
```

## Step 2: Find the place

```c++
std::pair<double, double> GetPosition(std::string input);
```

Given a location name, return the latitude and longitude. There is no duplicate location name. Mark the locations on the map. If the location does not exists, return (-1, -1).

Example:

Input: "ChickfilA" \
Output: (34.0167334, -118.2825307)

Input: "Ralphs" \
Output: (34.0317653, -118.2908339)

Input: "Target" \
Output: (34.0257016, -118.2843512)

```shell
2
**************************************************************
* 2. Find the position
**************************************************************

Please input a location:Target
*************************Results******************************
Latitude: 34.0257 Longitude: -118.284
**************************************************************
```

![](img/Target.png)

## Step 3: CalculateShortestPath

```c++
std::vector<std::string> CalculateShortestPath(std::string location1, std::string location2);
```

Given 2 locations A and B, find the best route from A to B. The distance between 2 points is the euclidean distance using latitude and longitude. You can use Dijkstra algorithm or A\* algorithm. Compare the time for the different methods. Show the routes on the map. If there is no path, please return empty vector.

Example:

Input: "Ralphs", "ChickfilA" \
Output: ["2578244375",
"5559640911",
"6787470571",
"6808093910",
"6808093913",
"6808093919",
"6816831441",
"6813405269",
"6816193784",
"6389467806",
"6816193783",
"123178876",
"2613117895",
"122719259",
"2613117861",
"6817230316",
"3642819026",
"6817230310",
"7811699597",
"5565967545",
"123318572",
"6813405206",
"6813379482",
"544672028",
"21306059",
"6813379476",
"6818390140",
"63068610",
"6818390143",
"7434941012",
"4015423966",
"5690152766",
"6813379440",
"6813379466",
"21306060",
"6813379469",
"6813379427",
"123005255",
"6807200376",
"6807200380",
"6813379451",
"6813379463",
"123327639",
"6813379460",
"4141790922",
"4015423963",
"1286136447",
"1286136422",
"4015423962",
"6813379494",
"63068643",
"6813379496",
"123241977",
"4399914059",
"4015372478",
"1732243576",
"6813379548",
"4015372476",
"4015372475",
"1732243620",
"4015372469",
"4015372463",
"6819179749",
"1732243544",
"6813405275",
"348121996",
"348121864",
"6813405280",
"1472141024",
"4015372462",
"6813411586",
"4015372458",
"6813411588",
"1837212101",
"6820935911",
"4547476733"]

```shell
3
**************************************************************
* 3. CalculateShortestPath
**************************************************************
*************************Results******************************
2578244375
5559640911
6787470571

...

6820935911
4547476733
**************************************************************
```

![](img/CalculateShortestPath.png)

## Step 4: The Travelling Trojan Problem (AKA Traveling salesman!)

```c++
std::vector<std::string> TravellingTrojan(std::vector<std::string> input);
```

In this section, we assume that a complete graph is given to you. That means each node is a neighbor of all other nodes.
Given a vector of location ids, assume every location can reach every location in the list (Complete graph. Do not care the neighbors).
Find the shortest route that covers all the locations and goes back to the start point. You can use either for the following heuristic (also discussed in the class):

- [2-opt heuristic](https://en.wikipedia.org/wiki/2-opt)

Show the routes on the map. For each intermediate solution, create a new plot. Your final video presentation should include the changes to your solution.

We will randomly select N points in the map and run your program.

```shell
4
**************************************************************
* 4. Traveling salesman problem
**************************************************************

TODO (Hong Shuo): Please make it clear that this is random. (both in menu text and in code)
Please input the number of the places:7
*************************Results******************************
6455606408
4019323628
269633700
2193435039
5757277355
269635546
6805760710
6455606408
**************************************************************
```

(TODO: Hong Shuo): make these smaller
![](img/Randompoints.png)

![](img/TravellingTrojan.png)

## Report and Rubrics:

Your final project should be checked into Github. The README of your project is your report.

### Report:

Your README file should include two sections:

1. High-level overview of your design (Use diagrams and pictures)
2. Detailed description of each function and its time complexity.
3. Discussion, conclusion, and lessons learned.

### Rubrics:

1. Implementation of auto complete: 10 points.
2. Implementation of GetPosition: 5 points.
3. Implementation of shortest path: 20 points.
4. Implementation of Travelling Trojan Heuristic: 25 points (either of the heuristics.).
   1. Animated plot: 10 points.
5. Creating reasonable unit tests: 20 points.
6. Video presentation and report: 10 points.
7. Extra credit items: Maximum of 20 points:
   1. A second shortest path algorithms (For example, you can implement both Bellman-Ford and Dijkstra): 10 points.
   2. 3-opt (If you chose to implement 2-opt for Travelling Trojan): 20 points.
8. [Genetic algorithm](https://www.geeksforgeeks.org/traveling-salesman-problem-using-genetic-algorithm/) implementation for Travelling Trojan.
