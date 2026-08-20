# Boris Cherny, Practical Tips on How to use ClaudeCode, June 2026

## CONFIG
/allowed-tools: customize tool
turn on macos dictation
How is @RoutingController.py used? => try @something @pathtofile

## COMMON WORKFLOW:
Explore > plan > confirm
Write tests > commit > code > iterate > commit: Red > Green > Refactor
Write code > screenshot result > Iterate: Implement [mock.png] Then screenshot it with Puppeteer and iterate till it looks like mock

## CONTEXT
Give claude context about the project, the system, history:
- claude.md: will be included in the context of all prompt in the project > keep it shorter possible: styleguide, command bash
=> ~/.claude/CLAUDE.md = GLOBAL (just me)
=> CLAUDE.md in the project (shared because you push it)
=> CLAUDE.local.md in the project (just me)

same thing with the settings.json
/memory pull down all the memory of the project

## KEYBINDINGS:
- shift+tab to auto-accept
- '#' to create a memory. For example: tests are written with vittest. (Where this rule is stored?)
- @ to add a file/folder to context
- ctrl + r for verbose output
- /vibe ??
