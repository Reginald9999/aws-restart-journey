# 🤖 AWS Lex Chatbot Project

## 🎯 Learning Objectives
- Understand the basics of **AWS Lex chatbot creation**
- Build a chatbot with **one intent and utterances** (Part 1)
- Extend the chatbot with a **quiz flow using branching logic** (Part 2)
- Demonstrate user interaction flow through **testing and presentation**

---

## ⏱ Duration
- Part 1: ~30 minutes
- Part 2: ~45–60 minutes

---

## 📌 Part 1 – Create a Simple Chatbot
### Step 1: Log in to AWS
- Navigate to **AWS Management Console**
- Search for **Lex** → Open **Amazon Lex**

### Step 2: Create Your Bot
- Click **Create bot**
- Name: `MyLexBot` (or any relevant name)
- Language: **English (ZA)**
- Advanced config: **None**
- Click **Create**

### Step 3: Add One Intent
- Go to **Intents → Create Intent**
- Name it: `S3Info`

### Step 4: Add Utterance
Example utterance:

### Step 5: Add Response
Example response:

### Step 6: Build & Test
- Save the intent
- Build the bot
- Test in the **test window** using the utterance

---

## 📌 Part 2 – Extend with Quiz Flow (Challenge)

### Scenario

I am working for an ed-tech startup **Cloud Learners Inc.**  
They want a chatbot that quizzes learners on **Amazon S3** knowledge.

### Step 1: Create Quiz Intent
- Create a new intent: `S3Quiz`
- Example utterances:
  - `Start quiz`
  - `Quiz me on S3`
  - `I’m ready for the quiz`

### Step 2: Add First Question
Bot response:

### Step 3: Add Branching Logic
- **Correct (A):**
  - Response: `Correct! S3 stands for Simple Storage Service.`
  - Follow-up: `Would you like the next question?`
- **Incorrect (B/C):**
  - Response: `Incorrect. The correct answer is Simple Storage Service. Would you like to try the next question?`

### Step 4: Add Next Question

- **Correct (A):** `Correct! Amazon S3 is primarily used for cloud storage.`
- **Incorrect:** `Incorrect. The correct answer is Cloud storage.`

### Step 5: Test the Quiz
- Use the Lex test window
- Try both **correct and incorrect answers**
- Ensure quiz flows to the **next question**

---

## ✅ Final Output
- **Part 1:** A chatbot that answers “What is S3?”
- **Part 2:** A chatbot quiz that:
  - Asks multiple-choice questions
  - Provides feedback for correct/incorrect answers
  - Encourages retry or progression

---
