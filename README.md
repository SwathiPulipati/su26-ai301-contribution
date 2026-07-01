# Contribution 1: [Issue Title]

**Contribution Number:** 1

**Student:** Swathi Pulipati 

**Issue:** [[GitHub issue link]](https://github.com/Listenarrs/Listenarr/issues/542#issuecomment-4618173632) 

**Status:** Phase III Complete

---

## Why I Chose This Issue

I chose this issue because it's part of a project that seems really interesting as a passion project.  It uses a tech stack that I have familiarity with, meaning the repository shouldn't be too difficult to work with.  In addition, the existing project seems to be an Ai-assisted project, so working on it will be a good opportunity to work with AI-written code using AI tools.  The issue itself is a fairly simple bug fix, but will require digging into the weeds of the code, which is a good starting point.  

---

## Understanding the Issue

### Problem Description

The total size of the audiobook given in the summary is not the sum of all the MP3 files that are part of that audiobook. Also, not every audiobook gives the total size in the summary.

### Expected Behavior

The total size of all files matched to the book should show accurately in the summary of the audiobook

### Current Behavior

Currently, it either doesn't show or shows the wrong value.

### Affected Components

Possibly the frontend, but likely the backend.

---

## Reproduction Process

### Environment Setup

My local environment was set up by installing node and .NET on WSL and then running the application.

### Steps to Reproduce (from Claude)

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install 20
nvm use 20

curl -fsSL https://dot.net/v1/dotnet-install.sh | bash -s -- --channel 10.0
echo 'export DOTNET_ROOT=$HOME/.dotnet' >> ~/.bashrc
echo 'export PATH=$PATH:$HOME/.dotnet' >> ~/.bashrc
source ~/.bashrc

npm install
npm run install:all
npm run dev
```


---

## Solution Approach

### Analysis

There are two problems here, the filesize being wrong and the filesize not appearing.

Wrong Value: Audiobook.FileSize (the field shown in the summary) is a legacy single-file field — it is never calculated as the sum of all files.
In RenameService.cs:379, when files are processed, FileSize is set to only the primary (first alphabetical) file's size.

No Value: Root cause: The frontend only renders the size field when audiobook.fileSize is truthy (AudiobookDetailView.vue:126)

### Proposed Solution

Dig into the code to find out where exactly the value is set and force it to recalculate everytime a file is added. It seems as though changing the calculation in AudiobookDToFactory.cs or LibraryController.cs could fix the problem.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** Audiobook size in summary is either missing or incorrect.

**Match:** Most other tags seem to be pulled from metadata rather than dynamic size, so there's nothing else like this.

**Plan:** [Step-by-step implementation plan]
1. Modify AudiobookDToFactory.cs to calculate filesize dynamically
2. If this does not work, modify LibraryController.cs to calculate filesize dynamically
3. If neither work, iterate with Claude Code until solution is found

**Implement:** Commit 7bd174cff30d145f4599b63ce7444772300b6939 (only on local currently, failing some remote push tests)

**Review:** 
1. Check to make sure it does actually work consistently
2. Make sure to follow code style guidelines.

**Evaluate:** I've downloaded a few audiobooks with varying characteristics to test whether this works for different cases.

---

## Testing Strategy

### Unit Tests

- [X] Test case 1: Downloaded audiobook 1 and tested manually importing the entire book together
- [X] Test case 2: Downloaded audiobook 2 and tests manually importing each file for the book separately
- [ ] Test case 3: Use automatic download through servers for audiobook 3 to test fully

---

## Implementation Notes

### Week 3 Progress

I believe I have found the cause of the bug and have made a solution for it.  More testing needs to be done to ensure the fix is resilient to the audiobook being uploaded from different sources. Currently failing some push requirements, so no code is on remote yet.

### Week 4 Progress

It seems as though the main repository has undergone many breaking changes in the days since I have been able to fix, so I must now come up with a different fix for the problem.  The entire codebase has been refactored such that none of my previous code is of any use.  I'm basically starting from scratch.  I'm reading through the codebase to understand the new structure and where changes may need to be made.  In addition, the new changes have brought to my attention that my environment setup may have been flawed so I am also working on fixing that.  Unfortunately, no personal changes have been pushed yet because of those environment problems.

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
