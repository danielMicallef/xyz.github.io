---
layout: post
title: I failed a live coding test. Here's why.
lead: And what I will do to never fail another one.
---

I successfully went through multiple interviews with this company and the plan was to meet their
customer, for whom I expected to start working. After a good number of questions, I was asked
to live code a very simple python problem: "Return True if every letter in an input string
appears exactly twice. Return False otherwise."

Test cases:
```
# "aabb" => True
# "abba" => True
# "aabbc" => False
# "aAbB" => False
```

I was not expecting a live coding test, and as customary with a group of interviewers looking at
you coding, I got excited. The first thing I thought is that it will probably be helpful if I simply
sort the input string. And so I did. I started writing without a proper plan. Once I had the string
as a list of sorted characters, I knew that I need to get every character, and make sure that the next
item in the list is the same. But the one after that must be different.

The result was messy and buggy.

```python
def duplicate_characters(input: str):
  """Returns True if each character appears twice"""
  input = sorted(input)
  for index, character in enumerate(input):
    next_index = index +1
    if (next_index % 2 != 1):
      continue

    if next_index > len(input):
        return False

    if input[next_index] == character and (next_index ==len(input) or input[index+2] != character):
      continue
    else:
      return False

  return True
```

## The Problem

The issue was not that I could not solve the problem. I was not using the right tools for the job. Exactly
after the interview was over I stopped to think about the problem and thought about using a map. And so I
wrote that down.

 > Count how many times each character appears, and store it in a map. Then make sure that every
character count is exactly `2`.

```python
def duplicate_characters(input: str):
    if len(input) % 2 != 0:
        return False

    char_count = {}
    for char in input:
        char_count[char] = char_count.get(char, 0) + 1


    if set(char_count.values()) != {2}:
        return False

    return True
```

This is a good improvement. It works! However on further inspection I realised that I'm only replicating the implementation
of `collections.Counter` There is no need to do the counting manually!

```python
from collections import Counter

def duplicate_characters(input: str):
    count = Counter(input)
    if set(count.values()) == {2}:
        return True

    return False
```

Making the function slightly more concise.

```python
from collections import Counter

def duplicate_characters(input: str):
    return set(Counter(input).values()) == {2}
```

 Short and easy to understand. Note that I also dropped the check for an even length at the start. The construction
 of `collections.Counter` takes `O(n)` time. Although slightly less efficient, on odd lengths (since calculating length
 is constant time `O(1)`), I went for a more concise function.

## How will I tackle a similar problem next time

I know all about Python and its different datatypes. I use collections, and itertools quite often. However I was
missing a framework on how to tackle such a problem. A framework which I could follow when excited and interviewers
are looking at every step. So the following is a first version of the thinking process I will use next time.

### 1. Understand the problem

Begin by reading the problem statement carefully and identify:
 - The Inputs (e.g. string, list of integers, etc)
 - Expected Outputs (e.g. Indices, boolean).

Write a function with type hints with inputs and expected outputs. Also write down a docstring explaining what the
function is expected to do.

! Hint: Think out loud whilst writing. This will let the interviewers clarify any misunderstanding before starting.

```python3
def only_contains_dupe_characters(input: str) -> bool:
  """Returns True if each character appears exactly twice."""
  pass
```

### 2. Choose the best Data Type

As we've seen above, selecting the best data type is critical, both for efficiency and to make the problem easier.
 - **List**: Ordered, mutable, allows duplicates. Use for collections that need modification, like adding or removing elements.
 - **Tuple**: Ordered, immutable, allows duplicates. Suitable for fixed data where order matters but changes are not needed.
 - **Dictionary**: Ordered (since Python 3.8+), mutable, key-value map. Ideal for fast lookups.
 - **Set**: Unordered, mutable, contains only unique elements. Perfect for checking membership or removing duplicates. O(1) average lookup time.

Ask questions:
 - Does order matter?
 - Are duplicates allowed?
 - Do I need fast lookups?

### 3. Decide if you can leverage something from Python's standard library.

 - Is Sorting needed? list.sort() takes `O(n log n)` time.
 - Consider datatypes from the [Collections module](https://docs.python.org/3/library/collections.html#module-collections)
   e.g `Counter`, `ChainMap`, `namedtuple`, `defaultdict` etc.
 - Consider helpers from [itertools module](https://docs.python.org/3/library/itertools.html#module-itertools)

### 4. Plan and implement your solution.

Break the problem into smaller subproblems and write skeleton functions. This will give you a clear path, and small managable
problems to solve under pressure.

### 5. Test and Debug

Use examples given by the interviewer to test that your solution matches expected outputs.

Add logs to debug any logical errors and issues.


## Managing Interview Stress and Focus

One key lesson from this experience is the importance of managing both stress and alertness during technical interviews.
Even if you know the solution, physiological stress can impact your performance. Dr. Andrew Huberman, a Stanford neuroscientist, provides evidence-based [breathing techniques](https://www.hubermanlab.com/newsletter/breathwork-protocols-for-health-focus-stress#breathing-for-stress-reduction) that can help:

### If You're Feeling Stressed or Overly Excited (like I was)
The "Physiological Sigh" can quickly calm your nervous system:
- Take two inhales through your nose
- Follow with one long exhale through your mouth
- Repeat 1-3 times
- This can be done subtly during the interview to reduce anxiety

### If You're Feeling Low Energy or Need to Focus
The "Alert Breath" can increase attention and alertness:
- Quick inhale through nose
- Short, sharp exhale through mouth
- Pace: About one breath cycle per second
- Do this for 30-60 seconds before the interview starts

These techniques, combined with the structured problem-solving framework outlined above, should help in tackling technical
interviews in the future. It's probably very normal to feel nervous or excited during live coding interviews. Having tools to
manage both your technical approach and your physiological state should make a significant difference in performance.
