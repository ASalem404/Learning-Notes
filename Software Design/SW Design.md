- Test-Driven Development Basics:

  - How is Classic TDD different from Mockist TDD?

    - What makes Classic TDD classic is the absence of mocking. We verify our classes or functions by testing them exactly as they occur without mocking out dependencies. This means that if some class we wish to test relied on the use of a database, we’d be testing that class with the database connection as well. While gives you a greater level of confidence that your code is working correctly, for code involving infrastructure code, it makes test setup and teardown harder and it makes them run slower as well slower

  - The rule of three:

    - The rule of three dictates that you should only clean up duplication when you come across it three times. Why?
      - You’ve probably heard of the DRY principle — do not repeat yourself. The idea is to remove duplicate code when you see it.
        This is a good thing to do because duplicated code can result in coupling and cohesion problems. It’s a good idea to refactor duplication into the appropriate abstraction to have a single place to maintain potential changes.
        However, the problem with cleaning up duplication immediately is that it is way easier to invent the wrong abstraction than it is for us to invent the right one.
        what we should do is wait. Let duplication occur twice for now. Yes, twice. A little bit of duplication is 10x better than an incorrect, complex, or wrong abstraction. However, once you see duplication three times, go ahead and refactor it.
        💡 Design rule: Remove duplication using The Rule of Three. When you see duplication for the third time, refactor it.
        💡 Tip: Refactoring using The Rule of Three applies to your test code as well as it does to your production code. For example, if you notice yourself doing the same setup code three times in your tests, think to refactor it out to a beforeEach abstraction in the test runner.

  - Write your tests based on behavior, not implementation:

    - When naming tests, keep in mind that we’d like to:
      - Use behavior-driven test names that mean something with respect to the domain:
        - If the domain has to do with fishing, use fishing terminology.
        - If you’re testing a calculator object, use mathematics terminology
      - Avoid using technical names and referring to primitives
      - Focus on what needs to happen instead of how it needs to happen.

    💡 Design rule: Write BDD-style tests using the language of the domain instead. Avoid technical names and leaking implementation details in test names.

    - Prefer concrete examples instead of abstract statements
      By providing lots of examples, we can evolve the design to understand what it is and what it does. Sometimes, we’re designing something that is based on abstract or novel concepts. Maybe the domain is something new to us. For better understandability, we should provide concrete examples.
      example:

      ```js
      // Instead of
      "can add two numbers";

      // Prefer
      "addition";
      "when I add 2 + 2, I get 4 back";
      "when I add -2 + 2, I get 0 back";
      ```

      By providing concrete examples, we end up with tests that are more easily readable, less abstract, and it’s more conducive to write potential edge case tests this way.
      💡 Design rule: Prefer evolving the design with concrete examples instead of abstract statements.

  - Triangulation:
    write additional tests that explore different aspects of the behavior you're trying to implement.
    These additional tests should guide the code to a more generalized solution.

    Red -> Green -> Triangulate -> Refactor

    1. Red:
       Start by writing a test that captures the essence of the behavior you want to implement. This test should fail initially because you haven't implemented the feature yet.
    2. Green:
       Write the minimal amount of code necessary to make the test pass. This might involve hardcoding a value or providing a very basic implementation.
    3. Triangulate:
       Write additional tests by repeat the two above steps that explore different aspects of the behavior you're trying to implement.
    4. Refactor:
       Once you have a set of passing tests and a more generalized solution, you can refactor the code to remove any duplication or improve its structure while keeping the tests passing.

  - Three likely challenges you’ll encounter:

    1. Not sure how to name the test:
       Sometimes we know exactly what it is that we want to do, but naming the test is tricky. This often occurs if we’re trying to come up with abstract, clean, generic test names. Instead, use concrete examples to guide test names.
    2. Not sure how to express the behavior:
       We know what to name the test, and we know how to prove that it’d work, but as for how to make it work, we have little idea.
       - A concept called the Transformation Priority Premise can give you plenty of hints for the simplest possible solution.
       - OO design patterns and OO principles also help.
    3. No idea how to prove correctness:
       Say we know how to express the behavior, and we can name the test, but as for actually proving that it works? We’re stuck.

       - Write the test backwards using the Arrange-Act-Assert test structure.

       - What is Arrange-Act-Assert ?
         It’s a way to organize your tests into three parts based on the same Given-When-Then structure.
         In the Arrange phase, we set up all of the variables and class instances we’re going to need to implement the test. In the Act phase, we plug everything together and act out the behavior. Finally, in the Assert phase, we verify that the behavior we wanted to happen has occurred. If it didn’t, the test should fail.
         example:

         ```js
         // Arrange (Given) - preconditions
         let palindromeChecker = new PalindromeChecker();

         // Act (When) - act out the behavior
         let result = palindromeChecker.isAPalindrome("mom");

         // Assert (Then) - postconditions
         expect(result).toBe(true);
         ```

  - Avoiding Impasses with the Transformation Priority Premise:
    💡 To turn a failing test green, we transform our design with small changes. But some transformations are better than others.
    Some pave the way to successfully implementing all the desired behavior, while others create impasses — deadlocks — that prevent us from continuing.

    In TDD, there are two ways code gets changed: through transformations and refactoring.
    Transformations are small code changes that modify how the code behaves. They also have the effect of directing the behavior of the code from being specific (especially when we use the Fake It strategy) to being more generic.

    For example, if we only had one test for an add(x, y) function, then we could simply Fake It with a hard-coded value, like so:

    ```js

     describe('add', () => {
     it('should return 25 when adding 10 + 5', () => {
     expect(add(10, 5)).toEqual(25); ✅
      })
      });

      // Implementation
      function add (x, y) {
         return 25;
       }

    ```

    But upon adding a subsequent example, we are inclined to transform the solution towards a more generic solution.

    ```js

     describe('add', () => {

     it('should return 25 when adding 10 + 5', () => {
     expect(add(10, 5)).toEqual(25); ✅
        })

     it('should return 30 when adding 10 + 20', () => {
     expect(add(10, 20)).toEqual(30); ✅
        })
     });

      // Implementation
      function add (x, y) {
         return x + y;
       }

    ```

    - How are transformations different from refactoring?
      Both transformations and refactoring change the code, but transformations are about adding new behavior, while refactoring is about keeping the behavior the same. Refactoring is merely about changing the structure.

    We can use the Fake It solution for the first few tests but when duplication appears, we must naturally switch to the Obvious Implementation.

    - Problems with obvious implementation:

      1. There is no universal obvious implementation for every test.
         Two developers using TDD to solve the same problem may design their tests and the resulting implementation in completely different ways. What’s obvious to one person may not be obvious to the next.
      2. Not all implementations are made equal.
         Let me ask you a question. Which transformation is better? A transformation where you have to completely rework 80% of the code already written against 15 existing tests or a transformation where you have to change one line of code and add two more? The answer is obvious. It’s the latter. Transformations that force us to rip apart much of what we’ve already written can be tricky, and often — they just don’t work. If we get stuck, we have to revert back to the last commit and try again.

    - 💡 Impasses/deadlocks: Stuck?
      If you’re not able to add the next test because of the way you’ve written code to pass previous tests, we call such a situation an impasse or a deadlock in TDD. You want to avoid this as much as possible.

    - Complex transformations — especially early ones — increase the likelihood of an impasse.
      If, in our second test, we jumped straight to using the Object Pool design pattern, well — there’s a good chance that’s not necessary. It’s probably likely that we could have continued Fake It for a little while longer or use much simpler transformations.

    - 💡 Design rule: To prevent impasses, always prefer the simplest possible transformation.

    - Transformation Priority Premise (TPP):
      Set of guidelines proposed by Robert C. Martin to help developers prioritize the order of transformations during the refactoring phase of Test-Driven Development (TDD). The premise suggests a hierarchy of transformations that should be applied to code when refactoring to improve its design.

      The transformations outlined in TPP are meant to guide developers in making incremental changes to code while keeping it in a consistent and correct state.
      The idea is to prioritize simpler transformations before more complex ones to ensure that the code remains functional at all times.

      By general, transform the code one step further each time you need if you can, dont skip a step and go next if this step will make you pass the test.

    - 💡 Design rule: When you find yourself stuck, try applying the next transformation in the TPP table.
      You don’t have to follow this to a tee, but keep it in the back of your mind as you go about implementing production code to pass a test.
