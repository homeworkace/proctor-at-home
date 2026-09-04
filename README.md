"Hey \<big corpo of your choice>, can I please have a mock interview?"  
"No, we already have a proctor at home."  
The proctor at home:

# proctor-at-home
Proctor at Home is a self-contained technical interview practice workflow that should be paired with an LLM.

- Each session, it sources a problem from a basket of sites such as LeetCode, GeeksforGeeks, and Codewars, then prepares answers, milestones, and questions to ask the learner.
- The learner is presented with the question in their preferred chat interface, while the prepared material stays hidden in a local file.
- Emulating interview-like conditions, the learner is encouraged to respond frequently with thoughts, questions, and draft answers; progress is tracked on each turn, and the agent is instructed to only reveal hints after some turn threshold.
- Each session is logged to track the learner's reasoning and performance, which are used to adapt future problem selection.

# I'm seeking help!

Please open an issue/PR if you'd like to suggest or offer help with something.
- Architecture that will allow the learner to read and solve problems anywhere, on any device. This necessitates the storing and I/O of the learner's data on the cloud.
- Scripts for various simple or housekeeping tasks. The agent should be able to call on them instead of manually running commands and introducing procedural inconsistencies.
  - Generating a random integer
  - Calculating average proficiencies over a sliding window of sessions
  - Creating, filling, validating and moving session logs
  - Fetching or scraping from websites
- For complex tasks, or for tasks that are awaiting a script implementation, stricter railroads to force desired behaviours.
  - Getting the agent to perform actual browser use instead of a simple web search
  - Reducing agent helpfulness
- Heuristic to integrate the oldest recent session into the running profile. The profile should not bloat with more sessions, and should not change too much from session to session. This will likely be a combination of scripts and forced structured responses.
- Compatibility for different harnesses, models, and platforms. For example, the browser use skill may be invoked differently between ChatGPT Desktop and Claude Code. Shell commands for script calling may also be different depending on platform (or even Google Drive integration).
- Heuristic to track time instead of turns. During testing, learners found it hard to decide when 2 minutes' worth of effort is up. The agent can be asked to consult the timestamp for each message, but the design is unforgiving to those who are attempting the problem while occupied/going through their day.
- Design to onboard and calibrate user skills.

# Installation

Proctor at Home is not an executable program. It is a set of instructions and persistent files intended to be placed in a directory accessible to an AI coding agent with filesystem and web access.

## 1. Download the project

Clone this repository:

```bash
git clone https://github.com/homeworkace/proctor-at-home.git
```

Alternatively, download the repository as a ZIP and extract it somewhere convenient.

The project is designed to be portable. The directory itself contains both the instructions needed to conduct an interview and the persistent state used to track the learner between sessions.

## 2. Create the working directories

In the project directory, create:

```text
current/
archive/
```

Copy `profile_template.txt` to:

```text
current/profile.txt
```

Do not otherwise modify the template files. `current/` will hold the learner's profile and recent session logs, while older session logs will automatically be moved to `archive/`.

A fresh installation should therefore resemble:

```text
proctor-at-home/
├── current/
│   └── profile.txt
├── archive/
├── 0_start_here.txt
├── 1_preparation.txt
├── 2_proctoring.txt
├── 3_evaluation.txt
├── profile_template.txt
├── session_template.txt
└── sources.txt
```

## 3. Give the agent access to the directory

Open the directory as a workspace/project in a compatible AI coding agent. The agent must be able to:

- read, create, edit, move and rename files within the project;
- browse the web to retrieve interview problems and their supporting information;
- execute simple local commands where required, such as generating a random number.

The learner should **not routinely browse `current/` or `archive/` while an interview is in progress**. Session logs contain the prepared solution, milestones, hints and other information deliberately withheld by the proctor.

This is not a security boundary—the files belong to you and remain accessible to you. It is simply part of the honour-system separation between learner and interviewer.

# Usage

## Starting a session

Start a fresh conversation with the agent from within the Proctor at Home project and give it the following prompt:

> Let us start a new session. Read and follow `0_start_here.txt`.

The phrase **"let us start a new session"** deliberately distinguishes an actual interview from conversations in which you are inspecting, discussing or modifying the workflow.

The agent will then prepare the session before presenting a problem. Depending on your history, it may take a daily or random problem from one of the configured sources, target an area that deserves further practice, or create a variant of a recent problem.

Preparation is intentionally hidden from the learner. Once complete, the agent will present only the interview question and the attempt begins.

## During the interview

Treat the conversation as you would a technical interview rather than an online judge.

You are encouraged to:

- ask questions about the problem and its constraints;
- explain your initial intuitions, including ones you are unsure about;
- reason through possible approaches aloud;
- discuss complexity and trade-offs;
- write and revise a solution;
- ask the interviewer questions when something is unclear.

The proctor tracks progress turn by turn. It prepares milestones beforehand and deliberately limits when hints can be given, so asking for help does not necessarily mean that help will immediately be available.

The proctor may also ask follow-up or curveball questions to test your understanding of concepts you introduce or to explore your reasoning. These are part of the interview rather than necessarily hints towards the solution.

Syntax is not the principal concern. A solution may be expressed as code, pseudocode or sufficiently unambiguous reasoning.

## Ending the attempt

When you want your current answer evaluated, say:

> lock it in

You may do this whether you believe you have solved the problem, have only a partial solution, or have decided that you are stuck.

The proctor will stop the attempt and evaluate both:

- the quality and efficiency of the solution you reached; and
- the process you demonstrated during the interview, including reasoning, clarification, communication, use of assistance and responses to follow-up questions.

It will then reveal its assessment and relevant information that was withheld during the attempt.

## Disputing an evaluation

If you believe the evaluation misrepresents something that happened during the interview, use the word:

> dispute

Explain what you believe should be reconsidered. The proctor can review the session record and amend its evaluation where appropriate.

Otherwise, the completed session log becomes part of your learning history.

### Between sessions

No manual bookkeeping is normally required.

Proctor at Home retains detailed logs for the most recent sessions. As they age out of the active window, their useful evidence is compacted into `current/profile.txt` and the original logs are moved into `archive/`.

The profile is a living assessment rather than a transcript. It records longer-term evidence about skills, problem-solving habits and interview performance, while recent logs provide more detailed context. This allows later sessions to become adaptive without requiring the agent to reread the learner's entire history.

To move your progress to another machine, copy the project directory—or at minimum its `current/` and `archive/` directories—to the new installation.

## Customising problem sources

`sources.txt` describes the problem sources the proctor knows how to navigate and how each may be used for general or targeted selection.

Additional sources can be added over time. A useful source entry should explain:

- where and how problems are exposed;
- how a blind daily or random selection can be made, if applicable;
- how targeted problems can be browsed or filtered;
- where specifications, hints, editorials or community clarifications can be found;
- any limitations the proctor should know before serving a problem.

General/random selection is intended to remain independent of the learner's strengths and weaknesses. Targeted selection is where the learner profile is deliberately allowed to influence the choice.
