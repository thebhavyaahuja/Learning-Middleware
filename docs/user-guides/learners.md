# Learner Guide

How to use Learning Middleware as a student to get the most out of personalized learning.

---

## Table of Contents
- [Getting Started](#getting-started)
- [Browsing Courses](#browsing-courses)
- [Enrolling in a Course](#enrolling-in-a-course)
- [Setting Your Preferences](#setting-your-preferences)
- [Learning with Modules](#learning-with-modules)
- [Taking Quizzes](#taking-quizzes)
- [Using the AI Tutor](#using-the-ai-tutor)
- [Tracking Your Progress](#tracking-your-progress)
- [Tips for Success](#tips-for-success)

---

## Getting Started

### Create Your Account

1. Go to **http://localhost:3000** (or your deployment URL)
2. Click **"Sign Up"**
3. Fill in your information:
   - **Name**: Your full name
   - **Email**: Will be your username
   - **Password**: At least 8 characters
   - **Education** (optional): Your academic background
   - **Interests** (optional): Topics you're interested in
4. Click **"Create Account"**

You'll be logged in automatically!

### First Login

When you log in, you'll see:
- **Dashboard**: Your enrolled courses and progress
- **Explore**: Browse available courses
- **Profile**: View and edit your information

---

## Browsing Courses

### Finding Courses

Click **"Explore"** in the navigation to see all available courses.

Each course card shows:
- **Course Name**: Title of the course
- **Instructor**: Who created it
- **Description**: What you'll learn
- **Prerequisites**: What you need to know first
- **Modules**: Number of learning modules
- **Target Audience**: Who it's designed for

### Filtering Courses

Use filters to find what you need:
- **By Topic**: Computer Science, Mathematics, etc.
- **By Level**: Beginner, Intermediate, Advanced
- **By Instructor**: Specific teacher

### Course Details

Click a course to see:
- Full description
- List of modules
- Learning objectives
- Time estimate
- Instructor bio

---

## Enrolling in a Course

### How to Enroll

1. **Find a course** you want to take
2. **Click "Enroll"**
3. **Confirm enrollment**

That's it! The course appears in your dashboard.

### What Happens When You Enroll?

Behind the scenes:
- ✅ You're added to the course roster
- ✅ Your progress tracking is initialized
- ✅ All modules are made available
- ✅ Your learning preferences are set to defaults

You can start immediately!

---

## Setting Your Preferences

### Why Preferences Matter

Your preferences tell the AI **how you learn best**. The system generates unique content just for you based on these settings.

### When to Set Preferences

You'll be asked to set preferences:
- **First time** you open any module in a course
- You can **change them anytime** in your profile

### The Three Preference Dimensions

#### 1. Detail Level

**How thorough should explanations be?**

- **Brief** ⚡
  - Quick, concise explanations
  - Key points only
  - Best for: Quick learners, review, time constraints

- **Moderate** ⚖️ (Recommended)
  - Balanced explanations with examples
  - Covers all concepts without overwhelming
  - Best for: Most learners, first-time learning

- **Detailed** 📚
  - Comprehensive, in-depth explanations
  - Full context and background
  - Best for: Deep learning, complex topics, thorough understanding

**Example:** Learning about variables in programming

*Brief:* "Variables store data. Declare with `let name = value`."

*Moderate:* "Variables are containers for storing data. In JavaScript, declare them with `let` or `const`. Example: `let age = 25` creates a variable named 'age' with value 25."

*Detailed:* "Variables are named containers that store data values in your program's memory. JavaScript offers three ways to declare variables: `let` for block-scoped variables you can reassign, `const` for constants that can't be reassigned, and `var` for function-scoped variables (legacy). When you write `let age = 25`, you're allocating memory space, giving it the label 'age', and storing the value 25 there..."

#### 2. Explanation Style

**How should concepts be taught?**

- **Examples-Heavy** 💡
  - Learn by seeing concrete examples
  - Lots of code snippets, demonstrations
  - Best for: Practical learners, beginners

- **Conceptual** 🧠
  - Learn through abstract principles
  - Theory-focused, big-picture thinking
  - Best for: Theoretical learners, advanced students

- **Practical** 🔧
  - Learn through real-world applications
  - Use cases, projects, scenarios
  - Best for: Applied learners, professionals

- **Visual** 🎨
  - Learn through diagrams and illustrations
  - Charts, graphs, visual metaphors
  - Best for: Visual learners

**Example:** Teaching sorting algorithms

*Examples-Heavy:*
```python
# Bubble Sort Example
arr = [64, 34, 25, 12, 22]
# Pass 1: Compare adjacent elements
# [34, 64, 25, 12, 22] - swapped 64 and 34
# [34, 25, 64, 12, 22] - swapped 64 and 25
# ...
```

*Conceptual:* "Bubble sort implements a comparison-based sorting paradigm by iteratively swapping adjacent elements that violate the desired order, causing larger elements to 'bubble up' to their correct positions."

*Practical:* "Use bubble sort when you need to sort a small list (< 50 items) where simplicity matters more than speed. In production, prefer quicksort or mergesort for large datasets."

*Visual:*
```
Initial:  [5][2][8][1]
Step 1:   [2][5][8][1]  Compare 5 and 2, swap
Step 2:   [2][5][8][1]  5 < 8, no swap
Step 3:   [2][5][1][8]  8 > 1, swap
...
```

#### 3. Language Complexity

**What level of technical terminology?**

- **Simple** 👶
  - Everyday language, minimal jargon
  - Analogies to familiar concepts
  - Best for: Beginners, non-technical backgrounds

- **Technical** 🎓
  - Domain-specific terminology
  - Precise, formal language
  - Best for: Advanced learners, professionals

- **Balanced** ⚖️ (Recommended)
  - Mix of both approaches
  - Terms defined when introduced
  - Best for: Most learners

**Example:** Explaining recursion

*Simple:* "Recursion is when a function calls itself, like Russian nesting dolls where each doll contains a smaller version of itself."

*Technical:* "Recursion is a computational technique where a function invokes itself with modified parameters, approaching a base case that terminates the recursive call stack."

*Balanced:* "Recursion is when a function calls itself to solve smaller versions of the same problem. Each call works on a simpler case until reaching a base case (the simplest version that can be solved directly)."

### Changing Preferences

Your preferences can be updated anytime:

1. Go to **Profile** → **Learning Preferences**
2. Adjust settings for any course
3. Click **"Save Changes"**

**Note**: Changing preferences will generate new content for modules you haven't completed yet. Already-completed modules keep their original content (you can reset them to regenerate).

---

## Learning with Modules

### Module Structure

Each module contains:
- **Title**: What you'll learn
- **Learning Objectives**: Specific goals
- **Content**: Personalized explanations
- **Quiz**: Test your understanding

### Reading Module Content

Your module content is **unique to you** — generated based on your preferences.

**Tips:**
- 📝 **Take notes**: Key concepts, questions, examples
- 🔖 **Use headings**: Navigate with the table of contents
- 💡 **Try examples**: Run code snippets, work through problems
- ❓ **Ask questions**: Use the AI tutor when confused

### Module Status

Modules have three states:
- **Not Started**: Haven't opened yet
- **In Progress**: Currently working on
- **Completed**: Finished quiz successfully

### Completing a Module

To complete a module:
1. **Read** the content thoroughly
2. **Take** the quiz
3. **Score** 70% or higher

You can:
- ↩️ **Revisit** completed modules anytime
- 🔄 **Retake** quizzes to improve your score
- ⏭️ **Skip ahead** if confident (quiz first)

---

## Taking Quizzes

### Quiz Format

All quizzes are **multiple choice**:
- 10-15 questions per module
- 4 options per question (A, B, C, D)
- 1 correct answer

### Quiz Taking Tips

**Before Starting:**
- ✅ Review module content
- ✅ Take notes on key concepts
- ✅ Try practice problems

**During the Quiz:**
- 📖 Read questions carefully
- 🤔 Eliminate obviously wrong answers
- 🎯 Choose the BEST answer (sometimes multiple seem right)
- ⏱️ No time limit — take your time

**After Submission:**
- ✅ Review correct answers
- ❌ Understand your mistakes
- 📚 Study explanations provided
- 🔄 Retake if below 70%

### Quiz Scoring

```
Score Calculation:
Correct Answers / Total Questions × 100 = Score

Examples:
9 / 10 = 90% ✅ Pass
7 / 10 = 70% ✅ Pass
6 / 10 = 60% ❌ Below passing
```

**Passing Grade**: 70%

**What if you don't pass?**
1. Review the explanations for wrong answers
2. Re-read relevant sections of the module
3. Retake the quiz (unlimited attempts)

### Quiz Questions Come From Your Materials

All quiz questions are generated from:
- ✅ The module content you read
- ✅ The instructor's course materials
- ✅ Learning objectives for the module

**This means:**
- Questions test what was actually taught
- Answers are always in the materials
- Reviewing the content helps!

---

## Using the AI Tutor

### What is the AI Tutor?

An intelligent chatbot that:
- Answers questions about course materials
- Explains concepts in different ways
- Provides examples and practice problems
- **Never** gives quiz answers directly

### How to Access

Click the **💬 Chat** button:
- In the navigation bar (any page)
- On module pages (course-specific help)

### Asking Good Questions

**Do:**
- ✅ "Explain binary search in simple terms"
- ✅ "Can you give an example of recursion?"
- ✅ "What's the difference between let and const?"
- ✅ "How would I use this in a real project?"

**Don't:**
- ❌ "What's the answer to question 5?"
- ❌ "Do my homework"
- ❌ "Write my code for me"

### Example Conversation

```
You: I don't understand how binary search works. Can you explain?

AI Tutor: Of course! Binary search is a way to find items in a sorted 
list by repeatedly dividing the search space in half.

Think of it like finding a word in a dictionary:
1. Open to the middle
2. Is your word before or after this page?
3. Eliminate half the dictionary
4. Repeat with the remaining half

According to the course materials (Chapter 3, page 42):
"Binary search achieves O(log n) time complexity by halving the 
search space with each comparison."

Would you like me to walk through a specific example?

You: Yes, please show me an example with numbers.

AI Tutor: Great! Let's find the number 7 in this sorted array:
[1, 3, 5, 7, 9, 11, 13]

Step 1: Check the middle (position 3, value 7)
...
```

### The AI Tutor References Materials

Answers include **citations**:
- "According to the textbook (page 42)..."
- "As explained in Module 2..."
- "The lecture notes state..."

This helps you:
- ✅ Verify information
- ✅ Find where to read more
- ✅ Trust the answers

### Limitations

The AI tutor:
- ❌ Won't give quiz answers
- ❌ Can only discuss course materials
- ❌ Might not know about recent events
- ✅ Admits when it doesn't know

---

## Tracking Your Progress

### Your Dashboard

The dashboard shows:
- **Enrolled Courses**: All your active courses
- **Progress Bars**: Completion percentage
- **Current Module**: Where you are in each course
- **Recent Activity**: Latest quizzes, completions

### Progress Metrics

For each course:
```
┌─────────────────────────────────────────┐
│  Introduction to Programming            │
│                                         │
│  Progress: ▓▓▓▓▓▓▓▓░░░░░░░ 60%        │
│                                         │
│  Modules: 6 / 10 completed              │
│  Quizzes: 6 / 10 passed                │
│  Average Score: 85%                     │
│                                         │
│  Current Module: Control Flow           │
│  [Continue Learning →]                  │
└─────────────────────────────────────────┘
```

### Module Progress

Click a course to see module-by-module breakdown:
```
Module 1: Variables           ✅ 95%
Module 2: Data Types          ✅ 88%
Module 3: Operators           ✅ 92%
Module 4: Control Flow        🔄 In Progress
Module 5: Functions           🔒 Locked
Module 6: Arrays              🔒 Locked
```

### Performance Analytics

View your learning patterns:
- 📊 **Quiz scores over time**: Are you improving?
- ⏱️ **Time per module**: How long are you spending?
- 🎯 **Strong topics**: What do you excel at?
- 📉 **Weak areas**: Where to focus study time?

---

## Tips for Success

### Learning Strategies

**1. Set a Schedule** ⏰
- Dedicate specific times for learning
- Consistency beats intensity
- Even 30 minutes daily helps!

**2. Take Notes** 📝
- Summarize key concepts in your own words
- Create examples that make sense to you
- Review notes before quizzes

**3. Use the AI Tutor** 💬
- Don't struggle alone
- Ask when concepts aren't clear
- Request different explanations

**4. Space Your Learning** 📅
- Don't cram all modules in one day
- Sleep helps consolidate knowledge
- Review previous modules periodically

**5. Practice Active Learning** 🎯
- Don't just read — do!
- Try examples, write code, solve problems
- Teach concepts to someone else

### Adjusting Your Preferences

**If content feels too easy:**
- ⬆️ Increase detail level
- ⬆️ Switch to technical language
- ⬆️ Try conceptual style

**If content feels too hard:**
- ⬇️ Decrease detail level
- ⬇️ Use simple language
- ⬇️ Request more examples

**If you're bored:**
- Try practical/visual styles
- Focus on applications
- Skip to quizzes to test yourself

**If you're overwhelmed:**
- Choose brief detail level
- Use examples-heavy style
- Take breaks between modules

### Getting Help

**Technical Issues:**
- 🐛 Report bugs on GitHub
- 📧 Email support
- 💬 Ask in community forums

**Learning Difficulties:**
- 💬 Use the AI tutor
- 📖 Review module content
- 👥 Form study groups
- 👨‍🏫 Contact your instructor

---

## Frequently Asked Questions

**Q: Can I change my preferences after starting a course?**  
A: Yes! Update them in your profile. New content will reflect changes, but completed modules keep their original content.

**Q: How many times can I retake a quiz?**  
A: Unlimited! Retake until you're satisfied with your score.

**Q: Do quiz retakes replace my score?**  
A: Yes, your highest score is always recorded.

**Q: Can I skip ahead to later modules?**  
A: Yes, all modules are available immediately. However, we recommend following the order for best learning progression.

**Q: Is the AI tutor available 24/7?**  
A: Yes! Chat anytime you need help.

**Q: Will the AI tutor give me quiz answers?**  
A: No. It will explain concepts and help you understand, but won't directly give answers to assessment questions.

**Q: How is my content different from other learners'?**  
A: Every learner gets unique module content based on their preferences. Quiz questions may also vary.

**Q: Can I download module content?**  
A: Currently, content is web-only. You can bookmark pages or take screenshots.

**Q: What happens if I unenroll from a course?**  
A: Your progress is saved. Re-enrolling will resume where you left off.

---

## Next Steps

Now that you know how to learn with the platform:
- 📖 **[Browse courses](http://localhost:3000/explore)** and enroll
- 💬 **[Try the AI tutor](http://localhost:3000/chat)** with a question
- 📊 **[Check your dashboard](http://localhost:3000/learner)** for progress

Happy learning! 🎓

---

*Last updated: November 8, 2025*
