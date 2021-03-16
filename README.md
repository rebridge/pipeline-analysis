# Complexity

In this workshop, you'll be implementing a simple static analysis for validating basic properties of code.

## Setup

### Before you get started

Import this as a notebook or clone this repo locally. Also, ensure you [install latest version of docable](https://github.com/ottomatica/docable-notebooks/blob/master/docs/install.md)!

```bash
docable-server import https://github.com/CSC-DevOps/Complexity
```

Install dependencies for our code.

```bash | {type: 'command', failed_when: 'exitCode!=0'}
npm install
```

## Static Analysis Concepts

### Why regex isn't enough?

For the same reason [why parsing html with regex is problematic](https://stackoverflow.com/a/1732454/547112).

> will ins​tantly transport a programmer's consciousness into a world of ceaseless screaming, he comes, the pestilent slithy regex-infection wil​l devour your HT​ML parser, application and existence for all time like Visual Basic only worse he comes he comes do not fi​ght he com̡e̶s, ̕h̵i​s un̨ho͞ly radiańcé destro҉ying all enli̍̈́̂̈́ghtenment, HTML tags lea͠ki̧n͘g fr̶ǫm ̡yo​͟ur eye͢s̸ ̛l̕ik͏e liq​uid pain, the song of re̸gular exp​ression parsing will exti​nguish the voices of mor​tal man from the sp​here I can see it can you see ̲͚̖͔̙î̩́t̲͎̩̱͔́̋̀ it is beautiful t​he final snuffing of the lie​s of Man ALL IS LOŚ͖̩͇̗̪̏̈́T ALL I​S LOST the pon̷y he comes he c̶̮omes he comes the ich​or permeates all MY FACE MY FACE ᵒh god no NO NOO̼O​O NΘ stop the an​*̶͑̾̾​̅ͫ͏̙̤g͇̫͛͆̾ͫ̑͆l͖͉̗̩̳̟̍ͫͥͨe̠̅s ͎a̧͈͖r̽̾̈́͒͑e n​ot rè̑ͧ̌aͨl̘̝̙̃ͤ͂̾̆ ZA̡͊͠͝LGΌ ISͮ̂҉̯͈͕̹̘̱ TO͇̹̺ͅƝ̴ȳ̳ TH̘Ë͖́̉ ͠P̯͍̭O̚​N̐Y̡ H̸̡̪̯ͨ͊̽̅̾̎Ȩ̬̩̾͛ͪ̈́̀́͘ ̶̧̨̱̹̭̯ͧ̾ͬC̷̙̲̝͖ͭ̏ͥͮ͟Oͮ͏̮̪̝͍M̲̖͊̒ͪͩͬ̚̚͜Ȇ̴̟̟͙̞ͩ͌͝S̨̥̫͎̭ͯ̿̔̀ͅ


For example, what if we just simply used a regex to detect message chains in code?
Here is a simple example that illustrates where things could go wrong.

```js | {type: 'script'}
let file = `
a.fairly.long.message.chains.with.lots.of.data.accessors;

message.
    b.
    c.
    d.
    z.
    t.
    chains();

console.log("Waiting.................");
`;

for( var line of file.split(/\n/g) )
{
    if( line.includes('.') ) {
        let mc = line.split(/[.]/g);
        if( mc.length > 4 ) {
            console.log(`[BUILD FAILED] Message chain detected with length ${mc.length}: ${line}` );
        }
    }
}
```

While we can detect one message chain, we miss one, and falsely detect another.

The short answer is that any implementation of a static analysis that results in *both* high false positives and high false negatives is going to incite an angry mob of programmers, who will insist the tool be removed from the CI pipeline.

We need a technique that is more sound, more precise, and frankly often easier to implement than a basket of regex. See [A Few Billion Lines of Code Later: Using Static Analysis to Find Bugs in the Real World](https://cacm.acm.org/magazines/2010/2/69354-a-few-billion-lines-of-code-later/fulltext)

##### Video
If you have not already watched this video, you can get more background about Esprima here:
Watch [5:00-14:30,28:00-34:00](https://www.youtube.com/watch?v=ACYZFkvq0Sk).

##### Interactive AST

<iframe id="serviceFrameSend" src="http://esprima.org/demo/parse.html?code=function%20functionName(%20node%20)%0A%7B%0A%09if(%20node.id%20)%0A%09%7B%0A%09%09return%20node.id.name%3B%0A%09%7D%0A%09return%20%22anon%20function%20%40%22%20%2B%20node.loc.start.line%3B%0A%7D" width="800" height="600"  frameborder="1"></iframe>

If you want to play with esprima in another browser tab, check out: http://esprima.org/demo/parse.html.

## Code Walkthrough

Two design patterns are of importance here:
* A [Builder pattern](https://en.wikipedia.org/wiki/Builder_pattern), which is used to build up state and then finally emit.
* A [Visitor pattern](https://en.wikipedia.org/wiki/Visitor_pattern), which is used to abstract the process of visiting a data structure such as abstract syntax tree (AST). The only input you have to provide is what action to perform at each node.



## Workshop

The repository contains a stub that parses a javascript file and visits each function. 

1. Run the program and print all the tokens in an ast.
   ```
      npm install
      node analysis.js
   ```

2. Do a simple calculation

   * **PackageComplexity**: The number of imports used in the file.
   * **StringCount**: The number of strings used in the file.
   * **ParameterCount**: The number of parameters for functions


3. Using multiple visitors.

   * **SimpleCyclomaticComplexity**: The number of if statements/loops + 1. For live example of a code complexity calculator, see [jscomplexity](http://jscomplexity.org/).

4. Advanced.

   * **MaxConditions**: The max number of conditions (boolean predicates inside decision node) in one if statement.
   * **MaxNestingDepth**: The max depth of scopes (nested ifs, loops, etc) -- this one is hard, only expect a few to get to do finish this one.



