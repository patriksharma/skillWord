
/**********************************************************/

## skill id : SKILL004WORD
## Name : Prateek Sharma 
## Target Platform :  MacOS , IOS
## Confluence LoginID : Problem: Level 4- Word Score
## Problem I solved : https://brians.atlassian.net/wiki/spaces/SKILLBUILD/pages/433029226/Problem+Level+4-+Word+Score

/**********************************************************/

## How to run the project ?
1. You must have latest Xcode version installed in your Mac-OS. 
2. Open the zip file. Click on `SKILL004WORD.xcworkspace` file and it will open a project in your Xcode window.
3. cocoapods have been used in the Project. To install cocoapds please use the instruction in below link : https://guides.cocoapods.org/using/getting-started.html
4. Once cocoapods installed in your system, You need to Install pods in your project or update pods. You need to open project folder in terminal window and run `pod Install` Command to upadate the pods used in project.
for example : cd/ path to project folder
`pod install or pod update`
5. . Third party libraries `pod 'SnapKit' pod 'DotsLoading'` used in the project. Snapkit is commonly used library to set constraints using code. Dots loading is a progress loading indicator library. 
libraries link :
https://github.com/SnapKit/SnapKit
https://github.com/WilliamMaybe/DotsLoadingView

6. Once you have an updated code Click Cmd+B to build project, on successfull build click on RUN or Play button in Xcode window. Open it on any Simulator or attachced device.

## How to Test the project ?
1. Once App is installed in your Simulator or device. You can click on Textfield window. You can add any Letter or Word inside it. It will scramble all available words greater then 3 and display in list.
2. You can collapse or expand any cell of the list by clicking on th header.
3. Each header contains the total points on that section and number of words
4. Total of points will be displayed in the bottom center of device.


## What approach taken?
1. we have a main view controller class `MainViewController.swift`. Class contains the view and objects. Input is taken by the textfield and displays result in the table view.
2. `UnScrambler.swift` this class contains the logic behind scrambling all availble words in the `txt` formatted files. 
3. Logic behind all caclation is stored in this method . I have used combination here, I'm getting  combinations of elements without repetition.  
3A. First I load the `words.txt` or any other txt file in this class. After file get loading I will make a Dictionary of strings mapped to a list of valid words with the set of letters. 
to change any other word file for testing you can go to `unscramlber.swift` class and change file name under below method
```
  private func loadWordsFromFile() {
        // You can change the test file here for any other text file testing 
        guard let allWordsUrl = Bundle.main.url(forResource: Default.WORDS_TEST_CASE, withExtension: ".txt")
```

3B. When user add any input for example `battlebot` is added as Input in Textfield. User click on Search button. I'm calling the below function at that time.   Below function get a Array fo all available words from text file, Calculate the points of each character and total points of each group. Check the comments added in the function inside `MainLogic/UnScrambler.swift`. file  
## How it solves the problem?
1. Method `func unscrambleWord(_ unscrambledWord: String) -> [TableViewSection] {` inside `UnScrambler.swift` is solving the problem. First we get a clean string by removing any white space or extra character. 
2. Then we check in for loop `Iterate from 3 -> Length of unscrambledWord`, As our minimum requirement of string is 3 char long.
3. The we store All possible combinations of words with length i in a Array Say `ArrayX`
3A.  How I'm getting all possible combination  ? 
```
            let wordCombinations = Combinatorics.combinationsWithoutRepetitionFrom(Array(sortedWord), taking: i)
```
Now this line is calling another class which contains a logig behind calcuating the combinations.`Combinations.swift` inside Util folder.  Algorithm is based on Swift default Framekwork. `Foundation Framework`. No Third party API or library is called to get this result. 

4. Now we run a for loop of `ArrayX`  and  Get possible words from sorted dictionary of words which was a word.txt file.
5. Now here we fetch each single word. Do a calulation of its points using function `func gettingValueForCharacter(word:String) -> Int{`
6. Points are calculated using `CharValue.swift` where switch method will assign value to each character and bonus points will be given based on number of character count in string. I have added a comment in the charvalue to understand the logic behind calculating the points. there is a score table in this file
7. NOW LOGIC BEHIND FILTER THE NUMBER : Our major goal is to find the highest number in every group (3,4,5 ..). First we sort an array of every group so the highest number array will be on top.  `func getTheHighestNumberWord(array: [Word]) -> Int    {` created this function inside `unscrammble.swift` 
this method get called after each iteration for  Length of unscrambledWord Array. For example I have 20 words with 3 character for input word `battle'. Now I will filter and store only those words in Another array which has no repetitive character or no tiles remain.  
8 .Below mentioned code in `unscrambler.swift` file has the main logic. 
First I get the highest score making word and keep it Array
Now I check if same word can be made from available tiles or not.
if not then i check what other word we can make it from available tiles. 
in the end I check if still we have avaialble tiles more than 2 then we can make another word of it or not. 
```
 if (count == 0 && anotherIteration == false){
                // Store the very first object with highest value to array
                sectionPoints += item.scrabbleScore
                sortedArr.append(item)
              
                /* take an array and store remaining character from the input word and stored character array. This way we have a seprate array of remaining character in string
                 */
                subtractArr =  removeCharFromArray(charArr: storeAllCharArr, inputArr: textInputArr)  }
               
                // Need to check if highest value word can be made again from left charcaters ?
                
            else if (checkIfSameWordPossiblilty(itemWord:lastStoredWord!.word) == true)
            {
                sectionPoints += item.scrabbleScore
                sortedArr.append(lastStoredWord!)
            }
            else{
                // we will check here does elements of item contains in remaining character array or not
                let allElemtsEqual = ContainsAllArray(charArr: storeAllCharArr, inputArr: subtractArr)
                
                // store to array if it contains true value
                if (allElemtsEqual == true){
                    sectionPoints += item.scrabbleScore
                    sortedArr.append(item)
                    subtractArr =  removeCharFromArray(charArr: storeAllCharArr, inputArr: subtractArr)
                }
                else{
                    // This bool value will break the infinite loop of checking duplicate words
                    iterationFailed = true
                }
            }
            count += 1
        }
        // LOGIC TO CHECK IF REMAINING CHARCATER ARE STILL GREATER THAT THE WORD COUNT OR DOES IT CONTAIN OTHER WORD WITH PREVIOUS ENTRIES.
        if (subtractArr.count >= wordCount && iterationFailed == false)
        {
            anotherIteration = true
            _ = getTheHighestNumberWord(array: array)
        }
        // lets make one more array with left char
        if (subtractArr.count > 2)
        {
            let   remainingChar  =  subtractArr.joined(separator:"")
            leftCharIteration = true
            _ = unscrambleWord(remainingChar)
            leftCharIteration = false
        }
```

## project structure 
1. Word Files contains inside `WordFiles` folder
2. MainLogic folder contains the `unscrambler.swift` file which contains the main logic or algorithm to find highest score. `TableviewSection and word.swift` file is an object file to store all data of words.
3. Util folder contains seprate files for color code, device type, font, icon or messages used in the project
4. Extension folder is used to add any direction extension the view or object for example changing color of button or UIview. 
5. `MainViewController.swift` files inside view controller folder holds a view objects of Window screen. It contains textfield, search button and display of result. 
6. `SKILL004WORDTests.swift` inside SKILL004WORDTests folder contains Unit test cases for the project. 
7. Assets.Xcassets contains images or App icon files.

## how to tally a Score points ?
Once you search any word and want to know the points calculation. Click on any of the word (row of table view )in Simulator. It will display you the score card and points calculation. 

![Image of scorecard](https://www.dropbox.com/s/6whdy2pb9yj9rj7/ScoreCard.png)



## Total Time : 60-70 hours to complete project. 


