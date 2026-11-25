# Hakoni founding discussion: from "Agent X" to Hakoni

**Question this document answers:** how did the Hakoni idea and its name come about, and what did we decide, as opposed to what the assistant proposed?

## Summary

In a voice discussion with a ChatGPT assistant, we develop one idea: send a single Git link, and let a general-purpose agent build a sandboxed, collaborative work environment, possibly a successor to spreadsheet-based business tooling. The project is to be its own first demonstrator. It is named *Agent X*, then *Seed*, then **Hakoni**; the discussion ends with logo drafts.

## Detailed summary

**Starting point**
- We want a public, emblematic agentic project. Our topic is agent packaging: OpenShell for security, and the container as a way to deliver an agent.
- The assistant frames it as "the container as the delivery unit of an AI agent", with a parallel to Docker standardising application distribution. This framing is the assistant's.

**Thesis**
- A large share of informal business IT runs on spreadsheets and hand-made tools (we give "90%" explicitly as a made-up figure). What replaces them? Our proposal: small specialised containers, powerful, independent of heavy infrastructure, and controllable. "Can we kill Excel?"
- The assistant adds: the challenge is to match the spreadsheet's ease of creating, changing and sharing; the agent hides installation complexity, so the user does not have to learn the technology that replaces the spreadsheet.

**Git as the distribution plane**
- The flow: send the onboarding link; the agent asks to install missing prerequisites (OpenShell, Docker), fetches the Dockerfile, builds the container through OpenShell, asks to connect over SSH, then inside it clones the repository, creates its branch, installs and works, and collaborates with others.
- Git also carries a work log and continuity: fetch at each new session to see what was missed.
- The assistant sums it up as five roles for Git: distribution, installation, collaboration, memory, continuity.

**Reflexive demonstrator**
- Develop the project with itself: paste exactly the same onboarding file into Codex or Claude Code and everything gets built. "It's a seed: we plant a small seed and everything grows."
- The assistant proposes a meetup demo: two machines, two clients, the same link.

**Naming**
- *Agent X*: our neutral placeholder.
- *Seed*: our proposal; the assistant adds Seed / Plant / Grow and the title "Seed — Can we finally kill Excel?". Our answer: "the meetup is very far off, but note it".
- We reject *Seed* as a name: not distinctive enough to search for. Assistant ideas (Seedifact, Seedware…), then *Xenonite* and *Thrum* from *Project Hail Mary*. We reject the distributed-intelligence angle: we want "a small autonomous society", a self-contained whole. Seed stays a working name.
- Terrarium → *hakoniwa* (箱庭, "miniature garden in a box"), offered by the assistant. We coin **Hakoni** and decide: "we'll go with Hakoni".
- Assistant checks, unverified here: no inappropriate connotation found; a Japanese author uses the pseudonym "HAKONI" and writes about AI, Claude Code and MCP; some manga use "Hakoniwa" in their titles; *Hakone* (箱根) is a town; *hako ni* (箱に) is correct, common Japanese for "in(to) the box"; Hakoni as a brand contraction is plausible, though no native speaker has confirmed it.

**Logo**
- Drafts inspired by a photo of a miniature garden in a closable metal box; the photo may come from the Japanese miniature-garden creator Kurumi Garden (unconfirmed).
- We keep the closable-box logo. We reject the "more tech" variants (circuit garden, chip, terminal symbol) as too figurative, keep the original, and ask for colour palettes applied to the same master SVG.
- Later, outside this discussion, we chose the contemporary Japan palette; that logo is now in `site/assets/hakoni-logo.svg`.

**Next step stated at the end:** start a dedicated Hakoni repository later, whose first goal is that Hakoni builds itself from its own onboarding seed.

## Verbatim

> Translated from a spoken French discussion between us and a ChatGPT assistant (voice mode, with some typed messages near the end). Date of the discussion: not recorded; exported on 2026-11-24.
>
> Edited for a public repository:
> - The earlier, unrelated part of the conversation is dropped.
> - The names of employers, people, internal projects and internal tools are removed or replaced with generic terms in square brackets.
> - Our descriptions of earlier experiments are left out.
>
> Voice mode often produced two versions of each answer (spoken and written): only the fuller one is kept. Filler turns ("I'm looking", "are you still there?") and side remarks addressed to other people are dropped, and long answers are lightly condensed. Section headings are added by the editor. The raw export is kept privately, outside the repository.

### § 1. Agent packaging as a public topic

**We:** I'm going to try to do something around what I've been working on: agent packaging, really. I work a lot with OpenShell for security. But also on using the container as a way of delivering. I think that could be interesting.

**Assistant:** It is a much more cross-cutting topic. The angle I would take is not just securing agents with OpenShell, but **the container as the delivery unit of an AI agent**. Many people know how to build an agent; but how do you hand it to another team, version it, guarantee its runtime environment and control its autonomy? Four dimensions: packaging (harness, tools, instructions, dependencies, context), security (isolation, permissions, network access, control of actions), delivery (distribution, versioning, deployment, updates), portability (separating the agent, its runtime and the model providers). There is a parallel with Docker: it did not invent applications, it standardised how they are distributed and run. The same could happen for agents: from a bundle of prompts and tools to a deliverable software artifact.

**We:** All the more so because I have some fun ideas for it. Do you have my ideas box here?

**Assistant:** Yes, it is the `Ideas/` folder of your [private ideas repository]. I suggest a separate folder for packaging, securing and distributing agents through containers. Dictate freely; we'll structure it gradually, without fixing an architecture.

**We:** First, let's give it a basic generic name, for a small folder. Something completely neutral: Agent X.

### § 2. The thesis: what will replace the spreadsheet?

**We:** [...] And that's my starting point: 80, let's say, to pick a silly number, 90% of IT until today was done through Excel sheets, things hacked together on a corner of a table in companies. What will replace them tomorrow? How can we offer a replacement that is powerful, independent of a big infrastructure, and controllable? That's my idea: replace the Excel sheet by offering these small specialised containers.

**Assistant:** I think this is the central idea. The agent becomes almost interchangeable; the container carries the specialisation. Excel succeeded because anyone could build a small system for their need and pass it to a colleague without an IT project. Agent X could take up that philosophy: a specialised, easily transmissible work unit; a natural-language interface; an isolated, controllable runtime; connections to the company's data and tools; collaboration with other agents and humans; no heavy new infrastructure for each need. Not replacing Excel as a spreadsheet, but replacing part of the informal computing it made possible. Possible meetup title: *Beyond Excel: Containerized Agents as the Next Unit of Business Computing.*

**We:** Could we make it... the thing is: are we going to kill Excel? People have been trying for 30 years and nobody has managed. Will we manage to kill Excel?

**Assistant:** You mean a real Excel killer, meant to replace Excel as the tool for creating and distributing small business applications?

**We:** Yeah, Excel killer.

**Assistant:** Then the main challenge is less technical power than ease of creation, change and sharing. Sending an Agent X should almost be as simple as sending an `.xlsx` file today, with no particular IT skills on the receiving side.

### § 3. Git and the onboarding link as the distribution path

**We:** That's exactly where Git gets interesting. I've made my Git repository. I send the onboarding link. And then everything speeds up. Codex or Claude Code receives the onboarding link, asks to install the dependencies, because "ah, I've never seen OpenShell; fine, I need an OpenShell". It will ask to install, say, Docker or something like that. Then it fetches the Dockerfile and builds the containers by calling OpenShell's commands. It asks to make the SSH connection to the container once it has started. Once it's started, it goes in over SSH. And there: connection to Git, cloning the repository, creating the branch, functional installation, and we start working inside the container, in the context of the Git repository. Not only can we work, we can collaborate with the other people. There's also an aspect that interests me: logging. We can log to the Git repository: "I did this, this happened", write up lessons learned, but also fetch updates. Each time a session restarts, I can do a Git fetch to see whether I've missed any episodes.

**Assistant:** That answers the historic problem of ease of distribution. The user does not need to know what Docker, OpenShell, SSH or Git are: they pass a link to their general-purpose agent, which installs the environment with the necessary authorisations. Git now has five roles: **distribution** (the link replaces the emailed Excel file), **installation** (the repository holds the build recipe), **collaboration** (branches and workspaces), **memory** (actions, findings, lessons) and **continuity** (catching up at each new session). You are not distributing a frozen copy but an entry point to a living, versioned, collaborative environment, and the agent hides the complexity of installation. Perhaps the big difference with earlier attempts to replace Excel: **the user no longer needs to learn the technology that replaces it.**

### § 4. The reflexive demonstrator: Agent X builds Agent X

**We:** And we could make something completely reflexive: the demonstrator project of Agent X is itself an Agent X. How do I develop Agent X? By taking my onboarding link and coding it in Codex or in Claude. Ideally we could paste exactly the same onboarding file into one or the other, and everything gets built. It's a seed. We plant a small seed and everything grows.

**Assistant:** That would make an excellent demonstrator: **Agent X builds Agent X**, dogfooding with a reflexive twist. From a seed, a simple Markdown file, the runtime is built, the workspace initialised, the agent joins the shared project and contributes to Agent X itself, and its contributions improve the next generations of the seed. For a meetup: two machines, two clients, Codex and Claude Code, exactly the same link; watch both environments being built, then both agents start collaborating. Every difficulty met during development becomes a product problem to solve.

### § 5. Naming: Seed, then the search for a distinctive name

**We:** Shouldn't we call it Seed, or something like that? I like the little seed.

**Assistant:** I even prefer **Seed** to Agent X: not a generic agent, but a small artifact from which a whole agentic work environment is built. The metaphor extends naturally: **Seed** for the transmissible onboarding file, **Plant** for installation, **Grow** for developing and enriching the environment. And a provocative meetup title: **"Seed — Can we finally kill Excel?"**

**We:** Yeah, the meetup is very far off, but go ahead, note it.

**We:** The problem with Seed as a name is that it doesn't discriminate. With TANSTAFLLM, you type it into Google and you find nothing until you land on us. But Seed is a generic name.

**Assistant:** Seed is a great concept but a poor identifier on the Internet. Keep the seed metaphor with an invented, distinctive word: Seedifact (seed + artifact), SeedOps, Seedogenesis, Seedplicator, Seedware. Or, as with TANSTAFLLM, a literary or science-fiction reference, maybe a recursive acronym. I have not checked whether these names are available.

**We:** In *Hail Mary*, what were the little rock people called, the ones who build things out of minerals?

**Assistant:** The **Eridians**, in Andy Weir's *Project Hail Mary*; Rocky is the one Ryland Grace meets. They come from the planet Erid and make an extremely strong material called **xenonite**.

**We:** Xenonite is pretty.

**Assistant:** It is rather far from the seed. We could call the project Xenonite and keep Seed for its onboarding artifact. We would still have to check whether Xenonite is already widely used in software.

**We:** What's the story of Rocky's people? What's their system of society?

**Assistant:** A very collaborative civilisation without a government in the human sense. They practise the **Thrum**: several Eridians gather and think together, reaching reasoning beyond each individual, while keeping their individuality. Thrum may be an even better reference: autonomous, specialised entities that come together to solve problems they could not solve alone, the distributed intelligence you are building.

**We:** Yeah, but that's not it. I'm not into distributed intelligence here. I'd rather be in the small autonomous society: we build a whole that is autonomous. I liked the Bobiverse, but no, I'm not going to go off into the Bobiverse.

**Assistant:** I see: not a collective intelligence but a **small, functional, autonomous entity**, with its tools, knowledge, rules, memory and ways of interacting with the outside. I would look towards autonomous colonies, ecosystems or terraforming. Von Neumann probes carry the means of their own replication, but for you replication is not the goal: it is growing a complete working environment from a small seed. The terrarium metaphor also appeals to me: a small, bounded, protected, fairly autonomous ecosystem that can still exchange with its surroundings.

**We:** Yeah, we'll stay with Seed for now.

### § 6. From terrarium to Hakoniwa, then Hakoni

**We:** What do you call a terrarium where you grow plants inside?

**Assistant:** A plant terrarium; when it is sealed and lives on its own water cycle, a closed terrarium, or a *bottle garden*. *Vivarium* is more general. I like the pairing Seed and Terrarium: the seed holds the building instructions, the terrarium is the isolated environment where it grows.

**We:** Are there words in other languages for terrarium? In Japanese, don't they have a beautiful name for terrarium?

**Assistant:** German *Flaschengarten*, Dutch *flessentuin*, Swedish *flaskträdgård* (bottle garden); Japanese *kokedama* (a plant grown in a moss ball) and **hakoniwa**, "miniature garden in a box". Hakoniwa is not exactly a terrarium, but the idea of a small world contained in a box fits your autonomous agentic environment well.

**We:** That's pretty. How do you spell it?

**Assistant:** H-A-K-O-N-I-W-A, 箱庭 in Japanese, pronounced "ha-ko-ni-wa". *Hako* means box and *niwa* garden: literally, a garden in a box.

**We:** Pretty. I'll do an image search myself... I see video games. I see miniature worlds. *Hakoniwa Sekai*, that's not bad.

**Assistant:** *Hakoniwa Sekai* evokes "a miniature world in a box", which fits our small autonomous software universe. Hakoniwa itself is a fairly common term in Japan.

**We:** Hakoni, then, the short form: what does it mean in Japanese?

**Assistant:** Hakoni on its own is not a usual short form of Hakoniwa. But *hako ni* (箱に) means "in the box" or "into the box", depending on context. Hakoni could be a nice invented name: short, easy to say, indirectly keeping the reference to the Japanese miniature garden, and more distinctive than Hakoniwa.

**We:** Well, we'll go with Hakoni.

### § 7. Checking the name

**We:** There isn't any inappropriate manga connotation?

**Assistant:** I found no sexual, vulgar or otherwise inappropriate connotation for Hakoni or Hakoniwa. Hakoniwa is also used in video games and in psychology (sandplay therapy). I did find a Japanese author who uses the pseudonym "HAKONI" and writes about AI, Claude Code and MCP: a small collision in our field, but not a problematic connotation. We can keep Hakoni as a working name and check its availability more widely before any publication.

**We:** I'm finding manga called "Hakoniwa something".

**Assistant:** Several, including one titled simply *Hakoniwa*, classified as erotic Boys' Love romance, but also fantasy, thrillers and ordinary romances. The Japanese term has no sexual connotation in itself; some works create the association. Choosing Hakoni rather than Hakoniwa keeps a little distance from that. I would keep the name.

**We:** And Hakone, with an E, what's that?

**Assistant:** A Japanese town about a hundred kilometres from Tokyo, near Mount Fuji, famous for hot springs and Lake Ashi; 箱根, pronounced "ha-ko-ne". Hakoni with an I is clearly more distinctive.

**We:** When I search Hakoni on Amazon, I land on Lumibricks (*"Loomy Bricks"* in the transcript), little lit-up models.

**Assistant:** Lumibricks, formerly Funwhole, is a brand of construction sets with built-in LED lighting. I see no direct commercial link with Hakoni; the search engine probably broadens towards miniature worlds. Amusingly, it is our metaphor again: small worlds built inside a box. No reason to drop Hakoni as a software project name.

**We:** That's it, that's it. *(About an image of a miniature garden in a box.)*

**Assistant:** A small living landscape, with its plants, rocks and own environment, in a box. The metaphor really works for Hakoni.

### § 8. The logo

**We:** Make me a very sober logo, two or three colours, no more, light, a bit zen, inspired by this photo.

*(The assistant produces a logo, then, on request, a vector SVG version with three natural colours, a transparent background and vector lettering, and PNG exports at 2048, 400 and 800 pixels.)*

**We:** Commit all that: the SVG version, what we said, and so on. The name Hakoni is not bad. And you can rename the folder.

**Assistant:** Committed in your [private ideas repository]. Hakoni is the working name, and the sandboxing part is consolidated: OpenShell as the technical foundation to explore; the container as the distribution unit and security perimeter; separating host installation from execution in the sandbox; permissions, secrets and network access; independence from Codex or Claude Code; Hakoni as its own first demonstrator. Sandboxing does not only secure the agent: it delimits the small autonomous world we want to distribute. The principles are documented; the implementation is still to do. The folder is renamed to Hakoni.

**We:** And we'll start a small Hakoni repository a bit later.

**Assistant:** When we start its dedicated repository, the first goal will be for Hakoni to build and develop itself from its own onboarding seed. No need to create the repository now.

**We:** *(Typed, with a new example image.)* Try to make an icon using this one as an example. I like the box that can close. [...] OK, I like your first version. Put the name Hakoni (*"Akani", corrected to "Akoni" in the transcript*) back, with the font you used before, and in vector. [...] Keep this logo. And the image that inspired it.

**Assistant:** This last logo is Hakoni's reference visual identity: the closable box, the miniature garden, the little house and natural colours. The original photograph of the miniature garden in its closable metal box is kept too, as the founding visual reference. Both are to go into the future repository's assets; they are not committed yet.

**We:** I also see Kurumi Garden. What is it?

**Assistant:** **Kurumi Garden** (くるみガーデン) is a Japanese creator of miniature gardens combining succulents, small houses and handmade objects, with a museum-workshop in Yokohama ([website](https://kurumigarden.shop/), [Instagram](https://www.instagram.com/kurumi_garden/)). *Kurumi* (胡桃) means walnut. The photograph that inspired our logo may come from her work, but I have not confirmed it.

**We:** Keep the reference.

### § 9. Is "Hakoni" a reasonable contraction in Japanese?

**We:** Break down the word hakoniwa.

**Assistant:** 箱 *hako* (box, case, container) + 庭 *niwa* (garden, yard): literally "garden in a box". Hakoni keeps all of *hako* and the first two letters of *niwa*; and *hako ni* (箱に) also means "in the box": a nice double meaning for an agentic container.

**We:** Is *hako ni* correct, and commonly used? Give an example.

**Assistant:** Yes: *hako* (box) + the particle *ni* (destination or location) is correct and common. 箱に入れる *hako ni ireru*, "put in the box"; 箱に入っている *hako ni haitte iru*, "be in the box"; 箱にしまう *hako ni shimau*, "put away in the box"; 箱に種をまく *hako ni tane o maku*, "sow seeds in the box", where *tane* (種) means seed. It is not a standalone noun, though: it is a grammatical phrase completed by the rest of the sentence.

**We:** Is the contraction Hakoni for Hakoniwa (*"aconit pour aconitoa"* in the transcript) ridiculous or not, in a real Japanese context?

**Assistant:** Not ridiculous. Japanese uses contractions a lot for products, brands and familiar words: Pokémon (Pocket Monsters), konbini (convenience store), sumaho (smartphone), pasokon (personal computer). Hakoni fits Japanese phonetics: はこに, three morae. A Japanese speaker without context will not necessarily think of hakoniwa; they may hear *hako ni*, "in the box", as a deliberately unfinished phrase. With a logo showing a garden in a box, the link becomes obvious. It is a brand neologism, not an established contraction, and I have not had this perception checked by a native speaker.

### § 10. Logo iterations and palettes

**We:** I showed the logo to Claude, which immediately made the link with hakoniwa and finds the concept interesting for an agent running in a container. But it finds it looks like a zen shop: you don't see the tech side. Try to bring out the tech side.

**Assistant:** Three directions: turn the garden floor into a circuit board whose traces evoke the paths of a Japanese garden; replace the little house with a computer component, a chip representing the agent; or give the box the look of a software container, with a discreet terminal symbol. I prefer the first: the garden stays recognisable and the technology becomes part of the landscape. Keep it zen, two or three colours: the contrast between nature and computing can give Hakoni its identity.

**We:** OK for the terminal symbol, but it's too figurative. I like staying with something very pared down. Maybe a change in the colours, in the colour scheme. And maybe we can replace the house with another element. Give me a few proposals.

*(The assistant produces three variants.)*

**We:** I still prefer the original logo. Do you have an SVG version of it? OK, keep that version. Make me three different palettes.

**We:** Stop. What strikes me is that you regenerated PNG images. You should take the SVG and change its palette: three different palettes, but you keep the SVG and compile the SVG to PNG.

**Assistant:** Exactly: keep the paths, proportions and typography of the original SVG and change only the colour values. Three palettes: **Tech** (midnight blue, desaturated cyan, ivory), **Terminal** (anthracite, mint green, light grey), **Contemporary Japan** (indigo, vermilion, off-white). But I cannot find the SVG in your repository.

**We:** We probably never generated the SVG of the logo with the lidded box. Take the lidded box first. Don't touch the image: create an SVG that matches it and redraws it element by element.

**Assistant:** *(After we send the logo again.)* I retraced it with separate vector paths: lid, box, house, plants, stones and lettering, transparent background, colours editable independently. It is a manual retrace, very close to the original. This SVG becomes the master file for all colour variants and PNG exports.

**We:** Apply the other palettes and make an image with all the variants. Make me a small HTML page or a small PDF.

**Assistant:** A small HTML sheet and an image with the four palettes: the original, Tech, Terminal and Contemporary Japan. The SVG paths are identical; only the colours change. My preference is Tech.

**We:** You can see the box behind the house, because the house is a bit transparent. Hide it: give the house a small background, in the same colour as the base colour.

**Assistant:** Fixed in the SVG: the house has an opaque background in the box's inner colour, applied to all four palettes. The rest of the drawing is unchanged.
