# Intro

I view LLMs as a research tool first and foremost and a code generation tool second.  LLMs also slightly unreliable.  So any output you care about should be validated by humans.  Also, if you want an operation to run more reliably, you should have LLM write you a script that does that operation rather than having an LLM do it directly.

I use Claude for my LLM, but pretty much everything I do should work with any LLM.

# Tools

- [Claude CLI](https://code.claude.com/docs/en/quickstart) installed through [Gocode](https://github.com/gdcorp-engineering/gocode-client)
- [Github CLI](https://cli.github.com/)

# Prompting Process

0. **Create a Git worktree (Optional)** - Most of my work is done in my main checkout, but if I already have something in progress on the main tree I will create a new one.  I also have a script that copies over needed untracked files (e.g. `node_modules`, `certs`, etc.) to the new repo so I don't have to wait for everything to install again.
1. **Ask Claude Questions** - If I am unfamiliar with the repo(s) I'm working on, I will ask it questions about how things are structured.  If the work involves tools/libraries I'm unfamiliar with, I will ask about those.  At the end of this process I should have a high level picture of how I'm going to implement the solution for the problem I'm trying to solve.
2. **Prompt Claude to Make Changes** - For this I will decide if the problem is SM, MD, LG, XL.
    - SM - I will just write the code my self since that is less work than prompting and evaluating the result
    - MD - Prompt Claude directly
    - LG - Use Claude planning mode
    - XL - A custom solution for the problem.  Maybe it is a large migration script. Maybe it is several agents working together or antagonistically. It will depend on the specific problem being solved.

    For all of my prompts I attempt to write the simpliest prompt possible.  Most are a single sentence. I find it easier to evaluate the result if I am asking for changes in smaller chunks.  Also, I find it to be less work to guide Claude to a solution if I do incrementally rather than producing a big prompt up front.
3. **Evaluate Output** - Once Claude has made changes, I review them.  If the problem is not adequately solved, I go back to Step 1 or Step 2 depending on if I know how to proceed to complete the solution.  I generally do not have agents on my personal machine do the evaluation.  I prefer to have agents set up for the project and run on PRs do this.  This way the results are more consistent and can be better customized to the project.
4. **Push Code** - Push code to Github so it can be evaluated by a combination of Humans and LLMs.
5. **Respond to PR Comments** - I have a skill `\pr-review-comments` that will summarized PR comments and offer suggestions about how to fix them.

# When To Reach for Claude Other Tools

When I reach for other Claude tools:

- **hooks** - I have never reached for this, but I probably add my script to copy files over for worktree creation.
- **skills** - When I'm asking Claude to a task repeatedly, so I don't have to type it over and over.
- **agents** - When a task is too big for the context on one agent.  Usually because this is an especially big task, or I want the agents to act antagonistically.

# MCPs

Since, search is what I'm primary using LLMs for, I have several MCPs added that make it easier get information form new sources:

- [Atlassian](https://claude.ai/directory/atlassian) - This makes it easier to look things up in Jira and Confluence
- [Chrome Devtools](https://code.claude.com/docs/en/chrome) - Makes it so Claude can acutally see what is being rendered in the browser.  Helpful for troubleshooting CSS issues, and provides a way to work around auth issues for backend services.
- **Elasticsearch** [Old](https://github.com/cr7258/elasticsearch-mcp-server) [New](https://www.elastic.co/docs/explore-analyze/ai-features/agent-builder/mcp-server) - You will need to add this for each Elasitic search instance you have logs in.  This makes it much easier to to analyse logs when erros occur. 
- [tdl](https://tdl.int.gdcorp.tools/docs/meta/ai/ht-use-mcp-server) - Easier to find information about tools used at Godaddy
- [uxcore](https://tdl.int.gdcorp.tools/docs/products/frameworks-languages/shared-component/uxcore/MCP) - Helps Claude find information about uxcore components