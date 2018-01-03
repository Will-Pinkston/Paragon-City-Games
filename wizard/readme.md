# Wizard

Wizard is a WordPress plugin for the www.paragoncitygames.com website's Game Wizard feature. Game Wizard seeks to suggest products similar to a customer's interests via a short questionnaire.

Specifically, the Game Wizard is specified to receive a list of filters (along with their possible values) and a list of games, where each game is given one or more values in relation to each filter. As the user progresses through the questionnaire, the list of games is filtered down by each of the user's answers - the final list of games is presented to the user after the questionnaire is completed.
Since certain filters are considered 'inclusive' and others 'exclusive,' (i.e. a game labeled "easy" would filter under "easy," "medium," "hard," etc. while a game labeled "party" would never be considered as any other type within that filter unless explicitly stated) the final list of games presented to the user is sorted, with the games most closely matching the search criteria preceding those who match more generally.

Wizard accomplishes this procedure efficiently and dynamically via a single-sort algorithm:
1. a .csv file is uploaded to Wizard, containing
* all filters, along with all values for each filter
* all games, along with their associated values in relation to the filters
2. Wizard reads the .csv file exactly once to create a sorted tree:
   1. parsing filter types and values generates keywords matching those values
   2. a tree structure is begun by creating a root node
      * each level away from the root will coincide with one of the filters, such that there may exist a leaf node (with height equal to the number of given filters) for every possible combination of filter values - i.e. every possible response to the questionnaire.
      * These leaf nodes are considered to represent lists of game suggestions that will be returned to the user
   3. as each game is read, it is checked for valid filter values
   4. if all values are correct, the game is placed in the tree relative to its filter values
      1. for each combination of one possible value from each filter, add the game to the tree's leaf node that matches that combination
         * if the leaf node does not yet exist, create all nodes necessary to reach it
         * leaf nodes store games such that they are sorted by closeness to the node's filters inherently
3. Wizard uses the sorted tree to create a directory of search responses as .txt files

Let f be the number of filters specified
Let v be the maximum number of values among all filters
Let g be the number of games
Let n be the total length of input, or the number of characers in the .csv file
Since the file is read only once (with no backtracking or re-reading) and worst-case tree creation requires only a constant increase of linear time (i.e. each game requires the creation of a unique leaf node, which requires the creation of between 1 and f nodes - where f is the number of 
