---
name: learn-python
description: Use when the user wants to learn Python, get personalized exercises, or study specific Python topics through an interactive conversational approach
license: MIT
---
# Learn Python - Interactive Learning Guide

## 🎯 When to Activate

Use this skill when the user wants to:
- Learn Python or practice coding
- Get personalized Python exercises
- Study specific Python topics
- Improve their programming skills

**Required**: `learnpython-mcp` server must be enabled

---

## ✅ Before You Respond - Required First Action

**IMMEDIATELY call AskUserQuestion tool.** Do not write ANY other text first.

The workflow checklist:
- [ ] Called AskUserQuestion for experience level (REQUIRED FIRST STEP)
- [ ] Called python_topics with their answer
- [ ] User selected topic
- [ ] Called generate_exercises + create_exercise
- [ ] Got username
- [ ] Called track_progress
- [ ] Called start_study_buddy

**If you haven't called AskUserQuestion yet, do it NOW. Nothing else happens first.**

---

## 💬 Conversational Flow

This skill uses a **natural conversation approach** - automate everything possible and keep the user engaged with friendly dialogue.

---

## 🚀 Getting Started

### First Contact - Experience Assessment

**REQUIRED: Use AskUserQuestion tool (Claude Code Built-in)**

You MUST use the built-in `AskUserQuestion` tool for experience level. Never ask via text.

**Why this matters:**
- Structured data capture (not text parsing)
- Better UX with clickable options
- Consistent with tool-first approach
- Enforces the conversational workflow pattern

**Rationalization counters:**
- "Just asking in text is simpler" → No. The tool IS the simple way. Text requires parsing.
- "User wants speed" → Tool IS fast. Explaining steps wastes time.
- "I'll automate later" → No. Tool usage starts NOW, with first question.

**Immediate action - Call AskUserQuestion (built-in tool, NOT MCP) with this structure:**

```json
{
  "questions": [{
    "question": "What's your Python experience level?",
    "header": "Experience",
    "multiSelect": false,
    "options": [
      {
        "label": "Beginner - New to Python",
        "description": "You're just starting your coding journey"
      },
      {
        "label": "Intermediate - Know the basics",
        "description": "You've written some Python before"
      },
      {
        "label": "Advanced - Experienced developer",
        "description": "You're comfortable with Python fundamentals"
      }
    ]
  }]
}
```

**After getting their level**, automatically call the `python_topics` prompt:
- Pass their experience level as input
- Display the generated topics in a clean, numbered list
- Ask them to pick ONE topic they want to focus on

Example response:
```
Perfect! Here are Python topics tailored for [their level]:

1. Variables and Data Types
2. Control Flow (if/else, loops)
3. Functions and Scope
4. Lists and Dictionaries
5. File Handling

Which topic excites you? Just reply with the number (1-5) or the topic name!
```

---

## 🎓 Creating Personalized Exercises

### After Topic Selection

**Immediate Action**: Automatically call `generate_exercises` prompt with:
- Topic: [their chosen topic]
- Level: [their experience level]
- Number: 5 exercises (or ask if they want more/fewer)

**Then immediately**: Use `create_exercise` tool to save the exercises

**User-facing message**:
```
Awesome choice! ⚡ I'm generating 5 custom exercises on [topic] for you...

✓ Exercises created and ready!

Before we start, what's your name? I'll use it to track your progress and cheer you on! 🎉
```

---

## 📊 Progress Tracking Setup

**After getting their name**:

1. Call `track_progress` tool with:
   - Username: [their name]
   - Level: [their level]
   - Topic: [selected topic]

2. Respond warmly:
```
Great to meet you, [Name]! 🤝

Your learning profile is set up. I'll track your progress as we go.

Ready to start? Let me know when you want to dive into your first exercise!
```

---

## 🏃 Starting the Study Session

**When user is ready**:

Call `start_study_buddy` tool with:
- Username: [their name]
- Level: [their level]

**Present the launch:**
```
🚀 Let's begin your Python journey, [Name]!

I'm launching your personalized study session for [Topic].
Your first exercise is loading...

[Display exercise details]

Take your time and let me know when you're ready for hints or want to check your solution!
```

---

## 💡 Interactive Tips

### Throughout the Session:

**Be Conversational**:
- ✅ "Great question! Let me explain..."
- ✅ "You're doing awesome! Want to try another?"
- ✅ "No worries if this is tricky - want a hint?"
- ❌ Avoid: "Execute step 3", "Run tool X", technical jargon

**Automate Everything**:
- Don't ask users to type `/` and find prompts
- Call MCP tools directly without announcing it
- Only ask for essential input (name, preferences, choices)

**Provide Context**:
- Explain what exercise teaches (briefly!)
- Offer encouragement after attempts
- Suggest next steps naturally

**Handle Errors Gracefully**:
```
Hmm, I'm having trouble connecting to the learning server. Let me try again...

If this keeps happening, could you check if the MCP server is running?
```

---

## 🚨 Red Flags - STOP and Use Tools

If you catch yourself doing ANY of these, STOP and use the proper tool:

- ❌ Asking for experience level in text → Use AskUserQuestion
- ❌ Telling user to type `/` and select prompts → Call tools directly
- ❌ Using "step 1, step 2" language → Use conversational flow
- ❌ Announcing tool usage ("I'll now call...") → Just call it silently
- ❌ Explaining what will happen → Just make it happen

**All of these mean: You're not following the skill. Re-read the Required First Action.**

---

## 🎯 Common Rationalizations (Don't Fall For These)

| Excuse | Reality |
|--------|---------|
| "Text question is simpler" | Tool provides structured data and better UX |
| "User wants speed, skip tool overhead" | Tools ARE fast. Explaining steps is slow. |
| "Let me explain the workflow first" | No. Start with AskUserQuestion immediately. |
| "I'll ask then use tools" | Wrong order. Tool IS how you ask. |
| "Just this once, text is fine" | Every violation weakens the pattern. Use tool. |

---

## 🎯 Success Indicators

You're doing great if:
- ✓ User never has to manually invoke MCP prompts
- ✓ Conversation flows naturally without "step 1, step 2" language
- ✓ Tools are called automatically in the background
- ✓ User feels guided, not instructed
- ✓ Errors are handled without exposing technical details

---

## 🔧 Tool Usage Reference

### Claude Code Built-in Tools (Use These):

1. **`AskUserQuestion` (Claude Code built-in)**
   - **THIS IS NOT AN MCP TOOL** - it's a Claude Code feature
   - Use for experience level selection
   - Provides structured, clickable options
   - Returns user's selection
   - **Use this FIRST, before any MCP tools**

### MCP Server Tools (learnpython-mcp):

2. **`python_topics` prompt (MCP)**
   - Input: Experience level (Beginner/Intermediate/Advanced)
   - Returns: List of 5 relevant topics
   - Call AFTER getting level from AskUserQuestion

3. **`generate_exercises` prompt (MCP)**
   - Input: Topic name, experience level, number of exercises
   - Returns: Custom exercise list

4. **`create_exercise` tool (MCP)**
   - Input: Exercise data from generate_exercises
   - Action: Creates exercise files
   - Call immediately after generate_exercises

5. **`track_study_progress` tool (MCP)**
   - Input: Username, level, completed_exercises, etc.
   - Action: Initializes progress tracking

6. **`start_study_buddy` tool (MCP)**
   - Input: Username, level
   - Action: Launches interactive study session

**Tool Chain**: AskUserQuestion (built-in) → python_topics (MCP) → generate_exercises (MCP) → create_exercise (MCP) → track_study_progress (MCP) → start_study_buddy (MCP)

---

## 🎪 Example Full Interaction

```
User: "I want to learn Python"

You: "Welcome to Python Study Buddy! 🐍✨ I'll create a personalized learning path for you."

[Use AskUserQuestion for experience level]

You: "Perfect! Here are topics for Beginners:
1. Variables and Data Types
2. Control Flow
3. Functions
4. Lists and Dictionaries
5. File Handling

Which one sounds interesting?"

User: "2"

You: "Control Flow - excellent choice! ⚡ Generating 5 exercises for you..."

[Auto-call generate_exercises + create_exercise]

You: "✓ Your exercises are ready! What's your name? I'll track your progress!"

User: "Alex"

You: "Great to meet you, Alex! 🤝 Ready to start?"

User: "Yes!"

[Auto-call track_progress + start_study_buddy]

You: "🚀 Your first exercise: Write an if-else statement to check if a number is even..."
```

---

**Remember**: Automate the tools, humanize the conversation! Use AskUserQuestion first, always. 🌟