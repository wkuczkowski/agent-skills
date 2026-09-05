# Delegation and review

## Give Claude a bounded contract

Include the repository path, current objective, permitted file/module ownership, existing changes to preserve, relevant context paths, expected outputs, and acceptance checks. Identify the next checkpoint if the task is too large for one run. Put agreed decisions in project documents; do not make Claude reconstruct the entire parent conversation.

Use a prompt shaped like this, with only fields the task needs:

```text
Work in /absolute/repository.
Objective: <observable result>.
Own: <files/modules>; preserve: <other ongoing changes>.
Read: <requirements, interfaces, relevant review evidence>.
Deliver: <implementation/report paths and required validation>.
Stop at: <completed scope or concrete blocker>.
Report changed files, checks actually run, unresolved failures and limitations.
```

A tool-free architecture consultation is advice based on supplied context, not verification of current APIs or the repository. A test report should say whether it used mocks, real components, or the user's actual environment.

## Separate implementation from acceptance

Keep one writer per overlapping file set. Parallel work is useful for independent modules or read-only reviews; shared interface changes need one canonical contract before either side proceeds. One orchestration owner dispatches and resumes each Claude run. A second observer may inspect logs but does not launch a replacement independently.

At a review checkpoint, record the diff or hashes of the files under review. Reviewers cite reproducible findings against that version. Return a compact correction brief to the same persisted session after its previous process has exited: defect, evidence, required behavior, and files it owns. Verify the correction rather than rerunning every earlier check without a new reason.

An inbox file is a handoff mechanism only if the prompt says when to read it. Writing a message there does not prove the active agent has consumed it. Urgent scope changes require confirmed delivery or a controlled stop, not a competing writer.

Keep the user's role/model choices for the current task. Do not turn this session's split of Claude implementation and Astra review into a universal rule. Respect an authorized model change when quota or another blocker prevents continuation.
