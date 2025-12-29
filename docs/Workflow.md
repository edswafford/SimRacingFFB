# Workflow

This document captures the steps used to develop the Racing Simulator Force Feedback application. The workflow combines **Kent Beck's Canon TDD** (behavior-driven development) with **John Ousterhout's Design Principles** (complexity management).

## Philosophy

- **TDD discovers behavior**: Tests drive what the system should do, revealing interfaces and requirements incrementally
- **Ousterhout refines design**: During refactoring, apply design principles to manage complexity and create deep modules
- **Requirements are constraints**: Keep requirements in mind to ensure we don't break them, but let behavior drive the design

## Start with the vision or goal of the application

A short one-line description that is intentionally vague.

## Assume that your initial choices or design decisions are likely wrong and you will need to start over or change directions

This idea comes from Dave Farley's video "What TDD Looks Like In A REAL PROJECT (With Code Walkthrough)" on the "Modern Software Engineering" YouTube channel.  The TDD process provides a safety net (the tests) that makes it easier to change code and revisit assumptions without fear of breaking things, thus steering the design over time rather than trying to intuit the perfect design from the start.

## Build the application in small vertical slices

The first iteration will be a bare-bones skeleton of the application. That way performance can be measured from the very beginning.

## Write Use Cases

Start with a couple of use cases and add more use cases as you need them or think of them. 

## Pick one Use Case to work on

Pick the easiest one.

## Extract 2–3 concrete core behaviors

Behaviors should be in the form of Given/When/Then scenarios. They need to be intentionally free of implementation details, architectural assumptions, and premature abstractions.

## Write a test from the behavior using Cannon TDD and Design Workflow

---

## Roles

### User Role

- Creates use cases
- Selects one use case
- Creates behaviors from the use case
- Reviews and updates behaviors suggested by AI
- Writes production code to make tests pass
- Performs refactoring
- Makes final decisions on design and implementation

### AI Role

- Assists with use case creation (when asked)
- Suggests behaviors from the chosen use case
- Generates test code from behaviors
- Reviews code for design red flags using Ousterhout's principles
- Provides design feedback during refactoring

---

## Workflow Steps

### Step 1: Use Case Creation

**Actor**: User (AI assists if requested)

Creation of use cases can occur anytime during the workflow. If you are in the middle of the current workflow iteration, do not switch to the new use case. Record the use case and then continue with the current workflow. If there are no use cases, start here.  

- Create use cases that describe what the system should do from a user's perspective
- Each use case is a narrow vertical slice of functionality
- Use cases go in: `docs/Use Cases/{UseCaseName}/Behaviors.md`
- Add the new use case to `docs/Use Cases/README.md` with a checkbox to track progress
- Example: "Driver feels resistance when turning into a corner"

**Output**: Use case folder with `Behaviors.md` file, updated `README.md`

---

### Step 2: Use Case Selection

**Actor**: User

This is where you start if you have use cases.

- Review available use cases in `docs/Use Cases/README.md`
- Select simplest/easiest use case to work on
- Focus on one use case at a time

**Output**: Selected use case

---

### Step 3: Behavior Extraction

**Actor**: AI suggests, User reviews/updates

- AI analyzes the use case and suggests 2-3 concrete core behaviors
- User reviews, updates, and refines the behaviors
- Behaviors are written in `Behaviors.md` in the use case folder
- Each behavior should be:
    - Concrete and observable
    - Testable
    - Independent of implementation details

**Format for Behaviors.md**:

```markdown
# Behaviors for [Use Case Name]

## Behavior 1: [Name]
Given: [initial state/conditions]
When: [action/event]
Then: [expected outcome]

## Behavior 2: [Name]
...
```

**Output**: Updated `Behaviors.md` with 2-3 concrete behaviors

---

### Step 4: Behavior Selection

**Actor**: User

- Review all behaviors for the selected use case
- Pick the simplest behavior to start with
- Consider: Which behavior forces the system to exist? Which teaches something meaningful?

**Output**: Selected behavior to implement

---

### Step 5: Test Generation

**Actor**: AI

- AI generates a concrete, runnable test from the selected behavior
- Test follows Given/When/Then structure
- Test is placed in `tests/UnitTests/` mirroring the clean architecture structure
- Test should:
    - Have clear setup, invocation, and assertions
    - Be minimal (just enough to fail for the right reason)
    - Use concrete, observable data (not abstractions)

**Test File Structure** (mirrors clean architecture):

```
tests/UnitTests/
  Domain/
    [DomainEntity]Tests.cs
  Application/
    [UseCase]Tests.cs
  Infrastructure/
    [Infrastructure]Tests.cs
```

**Output**: Test file with failing test

---

### Step 6: Test Review

**Actor**: User

- Review the generated test
- Update if needed:
    - Clarify test data
    - Adjust assertions
    - Improve test name
    - Ensure it matches the behavior intent
- Ensure test is concrete and observable

**Output**: Reviewed and approved test

---

### Step 7: Make Test Pass

**Actor**: User

- Write the minimal code needed to make the test pass
- Focus on making it work, not making it right
- Don't refactor yet (that's Step 8)
- Add new test items to the test list if discovered during implementation

**Output**: Passing test, minimal implementation

---

### Step 8: Optional Refactoring

**Actor**: User

- If the code needs improvement, refactor it
- Keep all tests passing
- Focus on:
    - Removing duplication
    - Improving readability
    - Simplifying logic
- Don't over-refactor (only what's necessary for this session)

**Output**: Refactored code (if needed), all tests still passing

---

### Step 9: Design Review

**Actor**: AI

- AI reviews the code using Ousterhout's design principles
- Checks for design red flags (see Design Review Criteria below)
- Provides specific, actionable feedback
- Focuses on:
    - Module depth (deep vs shallow)
    - Complexity management
    - Information hiding
    - Abstraction level

**Output**: Design review feedback with red flags and suggestions

---

### Step 10: Iterate

**Actor**: User

- Decide next action:
    - **Option A**: Write another test for the same behavior (if behavior has multiple test cases)
    - **Option B**: Move to the next behavior in the use case
    - **Option C**: Move to the next use case
- If design review found issues, address them before moving on (or add to technical debt list)

**Output**: Decision on next step, updated code if issues were addressed

---

## Design Review Criteria (Ousterhout's Principles)

### Deep vs Shallow Modules

**Deep Module**: Simple interface, powerful functionality

- ✅ Good: Module with 3 public methods that handle complex logic internally
- ❌ Red Flag: Module with 20 public methods, each doing trivial work

**Questions to Ask**:

- Does this module hide complexity behind a simple interface?
- Is the interface smaller than the implementation?
- Would a user of this module need to understand its internals?

### Complexity Management

**Goal**: Reduce cognitive load

**Red Flags**:

- ❌ Long methods (>20 lines without clear reason)
- ❌ Deep nesting (>3 levels)
- ❌ Too many parameters (>3-4)
- ❌ Magic numbers/strings (use named constants)
- ❌ Inconsistent naming
- ❌ Comments explaining "what" instead of "why"
- ❌ Duplication that increases complexity (not all duplication is bad)

**Questions to Ask**:

- Can I understand this code without reading other files?
- Is the complexity necessary, or can it be simplified?
- Would a new team member understand this quickly?

### Information Hiding

**Goal**: Hide implementation details, expose only what's necessary

**Red Flags**:

- ❌ Exposing internal data structures
- ❌ Leaking implementation details in public API
- ❌ Tight coupling between modules
- ❌ Violating encapsulation (public fields, etc.)

**Questions to Ask**:

- What does a caller need to know to use this?
- What can be hidden?
- Are we exposing too much?

### General-purpose vs Special-purpose

**Goal**: Right level of abstraction

**Red Flags**:

- ❌ Over-abstracting for a single use case
- ❌ Special-purpose code that should be general
- ❌ Premature generalization
- ❌ Copy-paste when a small abstraction would help

**Questions to Ask**:

- Is this used in multiple places? (If yes, consider generalizing)
- Is this abstraction used only once? (If yes, might be over-abstracted)
- Would adding a new similar case require duplicating code? (If yes, consider abstraction)

### Strategic vs Tactical Programming

**Goal**: Invest in design where it matters

**Red Flags**:

- ❌ Tactical: Quick hacks that will cause problems later
- ❌ Strategic: Over-engineering simple problems
- ❌ Not investing in areas that will change frequently

**Questions to Ask**:

- Will this code be touched frequently? (Invest more)
- Is this a one-off? (Tactical is OK)
- Is this in a critical path? (Invest more)

---

## Design Red Flags Checklist

When reviewing code, check for:

### Structure

- [ ] Shallow modules (many public methods, little functionality)
- [ ] Deep inheritance hierarchies (>3 levels)
- [ ] Circular dependencies
- [ ] God classes/objects (too many responsibilities)

### Complexity

- [ ] Methods longer than 20-30 lines
- [ ] More than 3-4 levels of nesting
- [ ] More than 3-4 method parameters
- [ ] Cyclomatic complexity > 10
- [ ] Magic numbers/strings without constants

### Naming

- [ ] Unclear or misleading names
- [ ] Inconsistent naming conventions
- [ ] Names that don't reveal intent
- [ ] Abbreviations that aren't standard

### Coupling

- [ ] Tight coupling between modules
- [ ] Dependencies on concrete classes instead of abstractions (when appropriate)
- [ ] Violations of dependency inversion

### Cohesion

- [ ] Classes/methods doing unrelated things
- [ ] Low cohesion (things that change together aren't together)

### Documentation

- [ ] Comments explaining "what" instead of "why"
- [ ] Outdated comments
- [ ] Missing documentation for complex logic

---

## File Structure

### Use Cases

```
docs/Use Cases/
  README.md
  {UseCaseName}/
    Behaviors.md
```

**Notes**:
- `README.md` tracks all use cases with checkboxes (like a todo list)
- Each use case has its own folder containing `Behaviors.md`
- See `docs/Use Cases/README.md` for instructions on adding and tracking use cases

**Example**:

```
docs/Use Cases/
  README.md
  Driver feels resistance when turning into a corner/
    Behaviors.md
```

### Tests (Mirror Clean Architecture)

```
tests/UnitTests/
  Domain/
    Entities/
      [Entity]Tests.cs
    ValueObjects/
      [ValueObject]Tests.cs
  Application/
    UseCases/
      [UseCase]Tests.cs
    Services/
      [Service]Tests.cs
  Infrastructure/
    [Infrastructure]Tests.cs
```

**Example**:

```
tests/UnitTests/
  Application/
    UseCases/
      GenerateForceFeedbackTests.cs
```

### Source Code (Clean Architecture)

```
src/
  Domain/
    Entities/
    ValueObjects/
    Interfaces/
  Application/
    UseCases/
    Services/
    Interfaces/
  Infrastructure/
    [External dependencies]
  UI/
    [Presentation layer]
```

---

## Example: Complete Flow

### TBD

**Note**: This example is a placeholder. It will be filled in after working through the first use case, providing a concrete walkthrough of the complete workflow from use case selection through implementation.

## Requirements Compliance

While behavior drives design, keep these requirements in mind:

1. Support multiple racing games
2. Support multiple racing steering wheels
3. Allow user configuration
4. Read telemetry from game
5. Generate force feedback signal
6. Send signal to wheel
7. Process in loop: read → generate → send

**How to ensure compliance**:

- When behaviors are extracted, verify they don't violate requirements
- When design decisions are made, check they support requirements
- Add behaviors that explicitly test requirement compliance

---

## References

- **Canon TDD**: `references/standards/Cannon-TDD.md`
- **Goals and Requirements**: `docs/goals.md`
- **Ousterhout's Design Principles**: "A Philosophy of Software Design" by John Ousterhout

---

## Notes

- This workflow is iterative and flexible
- Not every step requires AI involvement
- Design review can happen after multiple tests pass (batch review)
- The goal is working software with good design, not perfect design upfront
- Requirements are constraints, not drivers - let behavior drive design