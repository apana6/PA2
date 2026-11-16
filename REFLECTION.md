1. For every function you wrote (TODO 1 to 19), explain in your own words what it does and
   give its time complexity in terms of n, the number of scenes. One short paragraph each.

    1) The scene constructor stores the scene data and creates exactly the number of child pointer
   slots needed. It initializes every slot to nullptr. Its complexity is O(1) because the scene can have
   at most three choices 
   2) The scene destructor deletes every child scene, the choices array, and the scene data. Deleting 
   a child also deletes its descendants. Its complexity is O(n) for the number of scenes in that subtree.
   3) isEnding checks whether a scene has zero choices so its complexity is O(1).
   4) getChoice checks whether an index is valid and returns the child at that index so its complexity is O(1).
   5) setChoice checks whether an index is valid and stores a child pointer in that slot so its complexity is O(1). 
   6) countScenes recursively counts the current scene and every scene below it. In my story, it visits each scene once
   so its complexity is O(n).
   7) countEndings recursively checks each scene and counts the scenes with zero choices. It visits every scene once
   so its complexity is O(n).
   8) height recursively finds the tallest path from a scene to an ending. It checks every scene and child relationship 
   once so its complexity is O(n).
   9) clear deletes the root scene. The scene destructor then recursively deletes the entire story tree so its 
   complexity is O(n).
   10) load opens my story file, reads the starting HP, builds the story by calling readScene, and stores the root in 
   the tree. Since it loads every scene once, its complexity is O(n).
   11) readScene reads one scene and recursively reads all of its child scenes. It visits each scene in the subtree 
   once so its complexity is O(n).
   12) askChoice repeatedly reads input until the player enters a valid choice. With respect to the number of scenes,
   each attempt is O(1), although the total work depends on how many invalid attempts the player makes.
   13) play follows one path through my story and updates HP, loot, and inventory. The inventory search can take O(n) 
   in the worst case, and the player can visit O(n) scenes, so the worst-case complexity is O(n²).
   14) printStats calls the scene-count, ending-count, and height functions. Each one is O(n), so the total complexity
   is O(n).
   15) printSpoilers starts the recursive ending search at the root so its complexity is O(n).
   16) printEndings recursively visits the scenes and prints every ending with its path. It visits each scene once 
   so its complexity is O(n).
   17) greedyRun follows one path and chooses the next scene with the greatest immediate loot. Since each scene has at 
   most three choices, its complexity is O(n) in the worst case.
   18) bestLoot recursively checks every possible branch of my story and keeps the best surviving path. Each scene is 
   visited once, so its complexity is O(n).
   19) optimalRun calls bestLoot and prints the result. Its complexity is O(n).
   
2. Why does each scene allocate exactly numChoices slots instead of declaring Scene<T>*
   choices[3]? What does an ending hold in choices_, and why?

   Each scene allocates only the number of child pointer slots that it will actually need. Like if a scene with two 
choices receives two slots instead of wasting space on a fixed array of three. An ending has zero choices, so its 
choices_ pointer is a nullptr, and it does not allocate an array. This also follows the assignment requirement to use 
exact-size allocation.

3. Walk through your Scene destructor. What gets deleted, and in what order? What would
   go wrong if choices_ were deleted first?

   My destructor first deletes every child scene in the choices array. Each child destructor will recursively delete the 
scenes below it. After the children are deleted, the destructor deletes the choices array and deletes the scene's 
data. When doing this, the child scenes must be deleted before the choices array. If the array were deleted first, I
would lose the pointers to the child scenes, and they would leak memory.

4. Show the path greedy took and the path optimal took on your story, with the loot for each.
   Explain why greedy lost. If greedy did not lose, change your story until it does, and explain
   what you changed.

    In my story, the greedy version takes path 1-1-2. This means the player auditions for a solo, performs in front of Sue, 
and then chooses to confront Sue. The greedy version does this path because confronting Sue provides 3 immediate 
loot, while taking the situation like a champ provides only 2 loot. However, confronting Sue costs 7 HP. The player 
loses too much health and dies. The greedy path collects 7 total loot. The optimal version takes path 11111, which means
the player auditions for a solo, takes Sue's rejection like a champ and keeps singing and performs 
at the open mic. The path will collect 15 loot and the player survives. 

5. bestLoot tries every path, yet it runs in O(n) time. Explain why it is not exponential, using
   what you know about paths and leaves in a tree.

   Although my story has multiple possible paths, the tree does not repeat the same scene in different branches.
My longest path has five choices, but each scene has at most three children. bestLoot visits each scene once and 
recursively checks its children. Since each scene has at most three choices, the work at each scene is constant. This 
means the total work done is O(n) which is not exponential. 

6. Which List methods does your game use, and why? What does one search cost as the bag
   grows?

   My game uses search to check whether the player already owns an item, addFront to add a new item to the inventory, 
size to print the number of items, and print to display the inventory. The list's destructor also deletes the stored 
items when the game ends.