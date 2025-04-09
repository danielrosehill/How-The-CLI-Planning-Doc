# 🧰 How The CLI!? - A Blueprint

![Blueprint](https://img.shields.io/badge/Status-Blueprint-blue)

(A project blueprint for a CLI documentation and assistance system)

This blueprint contains a project plan for a CLI documentation and assistance system that I would love to work on. 

I'm open-sourcing this idea because while it's something I'd like to create and work on, I'm either not in the best position or don't have the time to fully implement it right now. I'm sharing it as I might work on it in the future, but anyone is welcome to take it and run with it. I reserve no intellectual property rights over the idea, although if you happen to implement it, some acknowledgment would be much appreciated.

Like many aspirational projects, this takes a backseat to the boring enterprise of earning money (and cleaning the apartment). But hopefully this will materialize at some point.

## 📑 Table of Contents

- [Why CLIs Still Exist](#wait--why-in-2025-do-clis-even-exist)
- [AI Makes CLIs Easier](#ai-makes-using-clis-easier)
- [The Memory Challenge](#am-i-dumb-or-programmers-just-geniuses)
- [Static Documentation Problems](#the-problem-with-static-cli-docs)
- [Potential Solutions](#scrape-and-rag)
- [The Blueprint](#the-eventual-idea)
  - [CLI Library](#cli-library)
  - [Documentation Scraping](#scrape-the-docs)
  - [Change Detection](#change-detection-and-update-handling)
  - [Agent System](#llm-and-agent-creation)
- [Stack Overview](#stack-overview)
- [Getting Started](#getting-started)
- [Potential Challenges](#potential-challenges)

## ⌨️ Wait ... Why, in 2025, Do CLIs Even Exist!?

CLIs are great!

Despite the fact that we live in an era of unprecedented technological advancement, the fact that many prefer CLIs (and TUIs) even when GUIs are available speaks to their enduring appeal.

Something, however, that has taken me two decades of daily Linux use to finally understand: developers' preference for CLIs isn't rooted in the kind of tech snobbery that I once thought it was. CLIs (and TUIs) peel away what's needed to get software working to its bare essential elements which, in turn, minimizes the chance that X will not work due to the user running (say) some obscure Linux distro.

CLIs aren't made because they allow people to amaze their friends with their terminal skills (okay, that's never happening). They're made because they reliably work and - in "mission-critical" stuff - reliability trumps flashy UI/UX every day of the week. 

## 🤖 AI Makes Using CLIs Easier

Once AI tools became commonplace and easy to use, they made it vastly easier to use CLIs.

Reading lengthy man pages is tedious at the best of times (moreso if you have ADHD!). But it's far easier and less daunting to just open up an AI chat interface and type:

`Hey .... how do you create a bucket with the B2 CLI again?`

This has changed how I use technology. I now ... shudder at the thought ... seek out CLIs. 

I'm engaged in an open-ended quest to replace as many GUIs as I can with their CLI equivalents. But wait ... how do you remember all those commands?!

## 🧠 Am I Dumb? Or Programmers Just Geniuses?

For years I wondered quietly how those who work with CLIs on a daily basis - maybe programmers - managed to remember all the commands. 

Every command line interface is sort of like a miniature language. 

CLIs contain a labyrinth of commands and subcommands that are generally documented in things called man pages (short for manual).

Generally speaking, they are also extremely demanding and unforgiving. They won't accept the parameters if they're provided in the wrong order. A single character will ruin the syntax of an otherwise good command.

I assumed that programmers were just some sort of special breed that had an uncanny ability to remember dozens or even hundreds of these. But then I thought about the programmers I actually knew in real life (John and Eoin if you ever read this, this is for you) and I doubted that. 

(*I worked with John and Eoin in my first job at Vconnecta/Ecanvasser. They are both wonderful people. Both gave generously of their time to help me out with tech things. Eoin's introducing me to the sl CLI was an act of memorable kindness!*)

I'm certain that there is the odd prodigy who can recite every command in every CLI. But the rest are humans of relatively ordinary cognition, like me. We are prone to forgetfulness and confusion. 

The methodologies for remembering CLI are really no different than those of trying to remember other pieces of information you commonly use, such as bus schedules: If you use them every day, they bed down quickly into your working memory. Otherwise you rely on collections of documents. There are programs such as Zeal (and many others like Dev Docs) which allow developers to curate their own miniature libraries of CLI docs. Sort of like miniature wikis. 

## 📚 The Problem With Static CLI Docs

Sadly there is a problem with the very idea of CLI docs being static. 

If you've used AI tools for programming in the past year - and if you're reading this there's a good chance that you have - then you're probably very familiar with an exasperating problem.

Large language models go through training periods and can't keep pace with the rate at which all technology, including command line interfaces, moves forward.  

They will have in their knowledge an outdated variant of a CLI or SDK or API which is no longer compliant. Hence the syntax doesn't work. Trying to get a CLI to work with old commands is like walking into a bar in Denmark and trying to order a drink in Japanese. 

So what to do?

## 🕸️ Scrape And RAG?

Where documentation is maintained in public facing repositories, the potential ways to keep an updated store of it become a lot more pragmatic. If documentation repositories can be refreshed automatically and then maintained in a vector store, then we can keep an up-to-date copy. 

However, even here we have problems! What happens if the project decides to move to a new way of publishing its documentation? This can happen. But it would mean that the old links are useless as they're no longer the up-to-date documentation. 

## 📝 The Eventual Idea

Here are some notes for an implementation that might seem over-engineered, but which is designed and thought of in order to try to mitigate as many of these problems as possible. 

### 📋 CLI Library 

Most people don't need to use the same CLIs.

Depending on their tech stack, you might have a few core CLIs that you need for your daily work, and perhaps a few that you add over time as your stack and career evolves. 

A good starting point would be to have the user state their list of CLIs. This can be maintained as a list. 

The next step would be providing the option for the user either to manually specify the documentation link, or to run a search in the application to attempt to find an indexable repository. 

This would cover the needs of both those who need to maintain a specific version of the documentation, for example those using a specific version of a CLI or program, while accommodating the more generous needs to just get the latest version of the docs. 

This process would require a search API integration. 

### 🕷️ Scrape The Docs

Capturing the documentation live would be a repetitive and wasteful process, so it makes more sense to use a crawling pipeline in order to ingest the documentation and refresh it periodically when required. 

This could be achieved using a combination of Firecrawl and a local vector database. The vector database maintains discrete collections for each pool of documentation, and Firecrawl periodically scrapes and updates the documentation storage locally. 

Having the documentation vectorized would also enhance the ability of the system to answer queries from semantic search. 

### 🔄 Change Detection And Update Handling

There's a question of how the system could know about changed documentation. A proactive system in which vendors use something like webhooks to notify the community in general of new documentation might be the most ideal. But this could also be automated through watching a documentation git repository. 

When the repository is updated, this could queue an automatic action to re-index the documentation and re-vectorize it in the local store. 

### 🧩 LLM And Agent Creation 

Finally, here is an idea for an implementation that would tie everything together very nicely. 

Every CLI generates a unique agent in the user interface. The agent is connected to the knowledge store for the particular CLI documentation in the vector database. 

A nice additional functionality would be the ability for the user to create meta-agents which are connected to multiple document collections.

The split would be particularly useful as on a day-to-day basis, developers could talk to their meta-agent while choosing to go into the unique agent only if they wanted to very carefully curate the knowledge that it was exposed to. This could perhaps be handled more eloquently with automatic orchestration and internal agent routing. 

The large language model which would power the interactions with the user could be a general-purpose conversational model. It could be backed by an instruction prompt to help the user directly and concisely so that it simply provides direct guidance on the right CLI commands to run in any given context. 

---

## 🛠️ Stack Overview

| Component | Purpose |
|-----------|---------|
| CLI Library | Maintains a personalized collection of CLI tools relevant to the user's tech stack |
| Documentation Crawler (Firecrawl) | Scrapes and indexes CLI documentation from official sources |
| Vector Database | Stores vectorized documentation for efficient semantic search |
| Change Detection System | Monitors documentation sources for updates and triggers re-indexing |
| LLM Integration | Powers the conversational interface for CLI assistance |
| Agent System | Creates specialized agents for each CLI and meta-agents for cross-CLI queries |
| User Interface | Provides a conversational interface for interacting with CLI agents |
| Search API | Enables finding documentation sources for new CLIs |
| Webhooks Handler | Processes notifications from documentation providers about updates |
| Repository Watcher | Monitors git repositories for documentation changes |

## 🚀 Getting Started

If you're interested in implementing this blueprint, here are some suggested first steps:

1. **Choose your tech stack**: Decide on the frameworks and technologies you'll use for each component
2. **Start small**: Begin with a proof of concept for a single CLI tool
3. **Build the crawler**: Implement the documentation scraping system for your test CLI
4. **Create the vector database**: Set up the storage system for the documentation
5. **Implement the LLM integration**: Connect to an LLM API (OpenAI, Anthropic, etc.)
6. **Develop the user interface**: Create a simple interface for interacting with the system

### Recommended Technologies

- **Vector Database**: Pinecone, Weaviate, or Qdrant
- **Web Crawler**: Firecrawl, Scrapy, or Playwright
- **LLM Integration**: OpenAI API, Anthropic Claude, or Llama 3
- **Frontend**: React, Vue, or Svelte
- **Backend**: Node.js, Python (FastAPI), or Go

## ⚠️ Potential Challenges

- **Documentation formats**: Different CLI tools document their commands in various formats
- **Rate limiting**: Web scraping may be limited by the documentation sites
- **LLM accuracy**: Ensuring the AI provides accurate command suggestions
- **Keeping up with changes**: Maintaining an effective change detection system
- **User experience**: Creating an intuitive interface for CLI assistance

## 📜 License

This blueprint is released under the MIT License. Feel free to use, modify, and distribute it as you see fit.

## 🙏 Acknowledgments

This idea was inspired by my personal struggles with remembering CLI commands and the potential for AI to make command-line interfaces more accessible to everyone.

---

*Last updated: April 2025*