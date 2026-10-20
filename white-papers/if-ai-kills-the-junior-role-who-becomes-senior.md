# AI Might Solve the Junior Developer Problem. Unfortunately, We Need Junior Developers.

*If AI kills the junior role, who becomes senior?*

**Status:** Working structure; prompts and research notes are guidance, not article prose.

**Author:** James Storr

**Created:** 18 October 2026

**Audience:** CTOs, engineering leaders, technical managers and people responsible for developing engineering talent.

## Writing convention

- **Purpose** and **Topics to cover** bullets are editorial guidance.
- James writes beneath each **Your draft** heading. Later editing should preserve his position, uncertainty and reflective-practitioner voice.
- Keep observations, explanations, risks, responses and research distinct; the learning mechanism is not proven.
- Use British English and a grounded, professional, conversational tone. James writes or approves any cheeky lines.
- Write the executive summary last.

## Working thesis

**Proposed argument to test:** AI can make coding faster while increasing review and rework when a change does not fit the application. If engineers repeatedly accept generated implementations without practising the reasoning behind placement, standards and trade-offs, organisations may weaken the route by which some junior engineers develop judgement. The response is to redesign AI-assisted delivery around visible understanding, supported practice and increasing ownership.

**Boundaries:** This is conditional, not a claim about all junior engineers, all AI-assisted work or immunity among senior engineers. The habit and learning mechanism is a hypothesis from James's observations. The longer-term talent effect remains a risk to investigate.

## Executive summary

**Purpose:** Give a compact account of the observed delivery problem, the conditional talent risk and the proposed practical response.

**Topics to cover:**

- Open with total delivery performance: generation speed can be lost in review, architectural correction and rework.
- State the distinction between locally good code and a change that belongs in the application.
- Summarise the possible learning and talent-pipeline consequences without claiming causation.
- Introduce the plan → understanding summary → implementation workflow and people-first support around it.
- State what leaders should measure and what evidence is still missing.

### Your draft

<!-- Write this last. -->

## 1. The pull request that was quick until review

**Purpose:** Open with a concrete, privacy-safe observation and establish why the issue matters now.

**Topics to cover:**

- James has seen some junior-authored, AI-assisted pull requests where the code is well written and works locally, but the application is wrong.
- Typical review concerns include local coding standards, responsibility placed in the wrong component, architectural inconsistency, maintainability and, in some cases, transactional concerns.
- Use the representative Spring Boot endpoint: permission-decision logic sits in the controller method, while James's team's architecture expects the controller to provide the interface or contract and a dedicated service or class to own permission policy.
- Make clear that the example works, contains no reported incident or transaction defect, and describes a local architectural standard rather than a universal Spring rule.
- Capture today's cost: implementation speed followed by review, restructuring and rework.

### Your draft

It’s interesting to see how quickly engineers have embraced the AI tooling that has arrived over the last two years. It reminds me of my children learning to operate the remote control before they could say twenty words. They soon knew which buttons produced the pictures and sounds they wanted, without understanding what half the other buttons were for. Familiarity with the controls comes remarkably quickly.

In engineering, the results of that familiarity are uneven. Some less experienced engineers are producing fantastic, well-engineered work. Others are producing code that works, but shows little evidence that the decisions behind it have been thought through. This isn’t exclusive to junior developers, but it is a pattern in the PRs I see.

Consider a PR produced super fast. The code is readable, the tests pass and the workflow is green. Look closer, though, and the change doesn’t follow the established coding standards. Logic sits in the wrong components, responsibilities are muddled, and a future change will require touching code that should never have needed to know about it. The implementation arrived quickly.

The saving is easy to see because it happens first. The bill arrives later, in review and rework, and often lands on someone more experienced. A task that looked cheaper to implement can become more expensive to deliver. Moving the cost onto a senior engineer’s calendar does make it rather easier to miss.

Experienced engineers generally produce better results with the same tooling. That is hardly surprising; we would expect experience to matter without AI in the picture too. They have spent years learning to question an approach, recognise a misplaced responsibility and think beyond whether the code runs. Those habits remain useful when AI is doing the typing.

What concerns me is how the junior engineer develops that experience. Working through a problem, making choices and discovering why some of them were poor has always been part of learning the job. If the tool lets someone reach an outcome without working through those decisions, where do they get that practice? Correcting the PR addresses the immediate change. My concern is whether the engineer is also becoming better equipped to make the next one.

Employers have a direct stake in closing that experience gap. When feedback becomes lasting understanding, the engineer can take on more responsibility with less supervision. When the same corrections keep coming back, the business keeps paying for them. More capable AI will change the decisions engineers need to make, but employers still need people who can apply it well and own the outcome. Developing that capability is part of making the investment in AI pay, and part of building the next generation of senior engineers.

In this paper, I want to explore that concern and what we can do about it: how we keep AI-assisted delivery moving while ensuring the people directing it continue to develop the judgement the work requires.


## 2. Good code is not yet a good change

**Purpose:** Explain what engineering judgement adds beyond functional correctness.

**Topics to cover:**

- Distinguish behaviour that meets the immediate requirement from a design that fits the system and can be changed coherently.
- Work through the controller example and a clearly hypothetical later policy change.
- Explain the reasons behind responsibility boundaries and standards, including when a deliberate departure might be justified.
- Use design literature, engineering guidance and code-review research for their appropriate purposes; none establishes AI-related learning harm.
- Close by distinguishing improved first-pass output from the developer exercising judgement, even when a skill or harness supplies the standards.

### Your draft

Mechanically sound code doesn’t always make a good change. It can compile, pass its tests and produce the required output while leaving the system harder to understand or change. Functionality and design deserve separate attention. Google’s published code-review guidance makes that distinction explicitly: it asks reviewers to assess overall design and integration with the system, alongside functionality, complexity and tests. That is engineering guidance, rather than an experiment, but it makes the quality bar clear. [Google, What to look for in a code review](https://google.github.io/eng-practices/review/reviewer/looking-for.html)

Consider the representative Spring Boot endpoint. Its controller checks the user’s permissions before calling a service. In a codebase whose established architecture assigns permission policy to a dedicated service or class, that method has taken on a responsibility intended to live elsewhere. The endpoint works. The question is whether the placement preserves the way this application divides its responsibilities. This is an example of a local architectural decision, not a universal rule about Spring controllers.

Now suppose the permission policy changes, and another endpoint needs to apply the same rule. If each controller contains its own version, both need attention and there is an opportunity for them to drift apart. If they delegate to the same policy component through a stable interface, a policy-only change can be contained there. Both versions could pass tests covering today’s expected behaviour. The difference becomes visible when we ask what tomorrow’s change would require. Architectural checks can enforce some boundaries too; a green set of behavioural tests alone does not establish maintainability.

There is a longstanding design rationale for this. In his 1972 paper on modular decomposition, David Parnas compared designs for the same system and showed how their boundaries affected which modules needed changing when particular decisions changed. It is a worked design analysis, not a measured prediction of savings in our codebase. Its relevance is the mechanism: responsibility placement determines which parts of a system need to know about a decision. [Parnas, 1972](https://doi.org/10.1145/361598.361623)

That gives coding standards and principles such as SOLID a practical purpose. They give us a way to reason about responsibilities and the consequences of changing them. Simply naming a principle is insufficient, and following a familiar pattern without considering the problem can introduce needless complexity. An engineer should be able to explain why the boundary helps here, which alternatives they considered and what would justify a different choice. A capable AI model can propose an implementation; the engineer needs to assess its fit with the application.

Research on code review also puts understanding at the centre. Bacchelli and Bird’s study at Microsoft combined observations, interviews, surveys and analysis of review comments. It found that understanding the code and change was central to review, with benefits including knowledge transfer and alternative solutions. This predates generative AI and does not establish harm from AI use. It supports treating review as a discussion of the change and its reasoning, beyond a search for defects. [Bacchelli and Bird, 2013](https://www.microsoft.com/en-us/research/publication/expectations-outcomes-and-challenges-of-modern-code-review/)

A useful experiment in your next PR review is to ask the developer to explain the choices behind the change, alongside identifying what needs correcting. Why does this logic belong here? Which existing standard informed the approach, and why does it apply? What did they question, change or reject before pressing the go-faster button? Their answers give you a view of the understanding behind the implementation.

An obvious response is to put those standards into reusable AI skills or a harness that supplies them to the agent. That could improve the first attempt: in our controller example, the agent would have the architectural context needed to place the permission policy in the expected component. Giving the tool better guidance is a sensible way to address the immediate quality problem.

But suppose it gets the implementation right first time. If the developer accepts it without understanding why it is right, the concern from the opening remains. The workflow has supplied the judgement, while the developer may have had little occasion to exercise their own. We may have improved the output while allowing another opportunity to practise to pass by. Over time, that risks leaving the muscles we will need in future underdeveloped. How do we improve the tooling while ensuring the person using it keeps learning?

## 3. The missing repetitions behind judgement

**Purpose:** Examine how judgement develops and whether accepting AI-generated decisions can reduce the practice that builds it.

**Topics to cover:**

- Start from James’s observation that feedback can improve the next attempt, while the improvement is sometimes short-lived.
- Develop the proposed link between repeated decision-making, feedback, habit and growing independence; preserve it as a hypothesis.
- Use Lee’s confidence survey and Shen and Tamkin’s coding experiment for the different questions they address; preserve the limits of each.
- Acknowledge that some difficulties may predate AI, and illustrate constructive AI use alongside the behaviours of concern.
- Distinguish experience with a particular task from seniority; experienced engineers also need to learn unfamiliar work.
- Examine retained understanding and transfer, not just successful completion. Avoid claiming greater harm among juniors without comparative evidence.
- Identify which repetitions are useful practice and which are low-value toil; connect this to the counterarguments in section 6.

### Your draft

Consider how a developer might traditionally have learned from that same endpoint. They would read the surrounding code, trace a similar request and decide where the permission logic belonged. An initial attempt might put it in the controller. In review, a colleague could explain why that created a second responsibility and ask them to work through an alternative. Moving the logic would mean understanding how the components interacted, then checking that the behaviour still held. On the next task, the developer would have a decision and its consequences to draw on. With useful feedback and repeated opportunities to apply it, something that first needed explaining could become something they recognised for themselves. The practice was in making a choice, examining it and carrying the lesson into another problem. Simply typing the code was never the whole of it.

Today, that same review can become a single instruction to AI: “Address the PR comments.” The code can improve without the developer ever working through why it needed changing. The same tool could support a different approach: the engineer explains the reviewer’s concern in their own words, asks AI to compare the designs, then checks the proposed correction against the standard. AI can still make the edit, while the engineer works through the reason for it.

There is an uncomfortable possibility here: improving model capability may make it easier to accept an implementation without assessing its design. As the output becomes more convincing, accepting it becomes more convenient. We can become more confident in the tool at precisely the point when we need to be deliberate about applying our own judgement. A useful counter-habit is to compare the proposal with an existing implementation or written standard, then explain any departure before accepting it.

There is some evidence consistent with that concern. Lee and colleagues surveyed 319 knowledge workers and found that greater confidence in generative AI was associated with less reported critical thinking, while greater confidence in their own ability was associated with more. This was a self-reported survey across knowledge work, not a study of junior developers or proof that skills deteriorate over time. It does, however, give us reason to examine how confidence in the tool affects the scrutiny we apply to its output. [Lee et al., 2025](https://www.microsoft.com/en-us/research/publication/the-impact-of-generative-ai-on-critical-thinking-self-reported-reductions-in-cognitive-effort-and-confidence-effects-from-a-survey-of-knowledge-workers/)

Shen and Tamkin examined learning more directly. In a randomised experiment, 52 programmers worked with an unfamiliar Python library, with or without AI assistance. The AI-assisted group scored lower on an immediate assessment of conceptual understanding, code reading and debugging; average time savings were not statistically significant. Explanation-seeking and conceptual questioning were associated with stronger learning outcomes, although those interaction styles were not randomly assigned. The sample included experienced programmers, and the assessment was immediate. It does not establish long-term skill loss or greater harm among juniors. [Shen and Tamkin, 2026](https://arxiv.org/html/2601.20245v2)

My concern for juniors is that they are still building the judgement we expect them to exercise. An experienced engineer may draw on years of decisions, mistakes and consequences when questioning AI’s approach. A junior has had fewer opportunities to build those reference points. If AI supplies both the initial solution and the response to review, they can get more work finished without necessarily becoming better equipped to judge the next piece. That is the gap I want to understand.

### Why the improvement doesn’t always stick

When these issues are raised and coaching follows, I see short-term improvement across experience levels. The engineer checks AI output more closely, questions the choices and pays more attention to the harnesses, agents and skills they use. But the improvement often fades. My suspicion is that it is simply too damn convenient to slip back. A deliberate check takes effort; accepting the next plausible answer is easy. A stronger sign of progress would be seeing that engineer recognise the same principle in a different task and explain how it applies, before a reviewer has to point it out.

The same developer might have struggled with these decisions without AI. A recurring mistake can reflect gaps in understanding that the tool did not create. My concern is that AI can make it easier to work around those gaps without addressing them. We should examine how the tool changes the behaviour, while leaving room for difficulties that were already there.

The temptation to accept an answer is also shaped by expectations around us. A sales pitch that presents AI as a replacement for engineers encourages a different conversation from one about helping them work better. If an employer turns that promise into a delivery expectation, time spent questioning a result can start to look like time the tooling was supposed to have saved.

On YouTube and social media, I see hundreds of videos promising extraordinary results from AI. A claim that you can recreate GTA 6 from a single prompt makes for a better thumbnail than a careful discussion of changing a mature codebase. But an impressive demo tells us little about whether the same approach will work reliably in an existing system. Experience gives you reasons to question that leap. When you are still developing that judgement, a constant stream of confident claims can be persuasive. It is easy to see how a less experienced engineer might get pulled into the hype.

When convincing output meets pressure to ship faster and messages that engineering experience matters less, can we really treat repeated acceptance as an individual failing? The engineer still owns the change. But we should also examine the conditions that make accepting the answer easier to justify than challenging it. Otherwise, we keep riding the same merry-go-round of PR comments and rework.

That leaves a conundrum: how do we give developers the practice that builds judgement while preserving the value AI can bring to delivery? Taking the tools away until a junior becomes a senior is hardly a workable answer. Employers have delivery commitments and a bottom line to consider. But if today’s speed comes at the expense of tomorrow’s capability, the business pays for that too. There has to be a middle ground, right?


## 4. Standards in the prompt are not standards in the person

**Purpose:** Examine what an AI harness can and cannot solve.

**Topics to cover:**

- Describe James's setup as he describes it: an AI harness references centrally maintained Markdown repositories in “GitHub Spaces”.
- The standards can be available and still go unused when a developer chooses a direct prompting route that bypasses the harness.
- Even successful harness compliance does not show that the developer has understood or internalised the standards.
- Make the root distinction explicit: directing the tool and developing the person are related but different responsibilities.
- Avoid a rigid split between “approved” and “unapproved” AI use; workflow labels such as agentic or pair programming do not demonstrate judgement by themselves.

### Your draft

When I started exploring how to use AI more effectively in my teams, my main focus was on helping it make better decisions. I worked backwards from the code delivered, through the tickets defining the work and the technical designs informing the backlog, all the way to the product requirements. The aim was to build up context at each stage, so that when AI generated the code, it had more than an isolated ticket to work from. I wanted fewer mistakes and a clearer connection between the original intent and what we delivered.

I built a harness around spec-driven development. It drew on what I call “strategic context”: documentation maintained in other repositories that described our platform and how we expected it to be developed. Architecture decision records explained decisions made by architects. Front-end and back-end development guidance set out our standards and practices.

Alongside that strategic context, the harness used agentic workflows to help product and engineering teams curate the context specific to each project. Those workflows connected the product requirements, technical designs and delivery tickets, with the aim of preserving intent from conception through to delivery. We also had forums where we explored how to mature the tooling and improve the outcomes.

Velocity went up, and the measures we had in place were green. A success! On paper, everything looked rather good.

As time went on, though, I began to see the cracks. The approach worked well when a documented standard covered the decision. It was less dependable when the work moved beyond that guidance.

A development challenge might require a new design, an unfamiliar requirement or more involved logic than the existing examples covered. The harness could supply the context we had documented, but that context did not settle the new questions. AI could still propose a plausible implementation. Sometimes the approach fitted the system; sometimes it did not. A confident answer did not tell us which one we had.

When it went wrong, meetings followed. Engineers, architects and product managers would work back through the decisions, asking why a particular approach had been taken and why an issue had surfaced so late. “Because AI told me so” would hardly have been a satisfying explanation. The question was what reasoning the engineer could offer beyond the recommendation itself.

Those gaps exposed a limit to the approach. We could add more documentation after each problem, but we also needed engineers who could recognise when the existing guidance was insufficient, question a proposal and work through the unfamiliar parts. Even where the standard did cover the decision, a compliant result did not tell us whether the developer understood why it applied. The harness could guide the implementation; developing that understanding still needed deliberate attention.

### The human side of clarification

You may recognise the intent in approaches such as [GitHub’s Spec Kit](https://github.com/github/spec-kit/blob/main/templates/commands/clarify.md), [OpenSpec](https://github.com/Fission-AI/OpenSpec/blob/main/docs/faq.md) and [AWS’s AI-DLC](https://aws.amazon.com/blogs/devops/building-with-ai-dlc-using-amazon-q-developer/). Their workflows differ, but each provides ways to clarify what should be built before implementation. Asking the engineer to resolve ambiguity is a sensible response to an agent making assumptions on their behalf.

But a workflow can still turn clarification into a barrage of questions. As mental fatigue builds, each new question can start to feel like another hurdle to clear. The urge to choose the first plausible answer and press Enter can overtake the effort to think through its implications. We can end up measuring completion of the interaction while assuming the thinking happened along the way.

Research on questionnaires offers a useful caution. Galesic and Bosnjak varied question order in a web survey and found that questions appearing later received quicker responses, shorter written answers and more uniform responses to grouped items. This was survey research, not a study of engineering decisions or AI harnesses. It gives us reason to question whether sustained questioning reliably produces sustained engagement. [Galesic and Bosnjak, 2009](https://doi.org/10.1093/poq/nfp031)

Engineering decisions can require time to read the surrounding code, trace a dependency, compare alternatives or consider what a future change would involve. A rapid sequence of prompts, especially with colleagues waiting for an answer, can make it tempting to treat the interaction like a video game: respond, advance, repeat. The workflow needs to leave room to investigate a question, discuss it or admit that the answer is not yet known.

An experienced engineer may have relevant patterns and previous mistakes to draw on. Someone still building that experience may need help understanding the options before they can choose meaningfully between them. More time alone does not supply that understanding, and experience does not make anyone immune to accepting a plausible answer too quickly.

Research on AI-assisted decisions suggests that interaction design matters. Buçinca and colleagues ran an experiment in which 199 participants used simulated AI advice to choose food substitutions. They tested ways of encouraging participants to consider the decision themselves, such as asking them to choose before seeing the AI’s recommendation. Compared with presenting the advice and an explanation immediately, these approaches reduced how often participants followed incorrect recommendations. The designs that reduced overreliance most were also rated less favourably. This was a short experiment outside software engineering; it does not establish how a clarification workflow affects learning. It does show why ease of use and effective human scrutiny need separate evaluation. [Buçinca et al., 2021](https://arxiv.org/html/2102.09692)

The problem, then, is a gap in how we use and assess these harnesses. We can supply standards, preserve project context and require clarification, yet still know very little about the judgement exercised by the person using them. Completing the workflow can become a substitute for examining the decisions it contains. We risk moving passive acceptance from the generated code to the plan that produces it.

Before we can place greater confidence in the outcomes, we need to see that relevant context has actually informed the change, that assumptions have been examined and that unfamiliar decisions receive appropriate scrutiny. The workflow must allow someone to pause, investigate or seek help when the existing guidance is insufficient. An engineer should be able to explain why the approach fits the system and what they questioned or changed. Another explanation generated by AI and accepted unread would leave us with the same problem one stage earlier.

For junior engineers, that means making room for supported practice within delivery. They need opportunities to work through choices, compare alternatives with a colleague and apply review feedback themselves, even when AI performs much of the implementation. The amount of support should reflect the task and the engineer’s experience. A completed questionnaire cannot tell us whether that learning has happened; seeing the engineer recognise and apply the same principle on a later task would give us something more useful to assess.

Confidence in the approach should therefore rest on both the quality of the changes we deliver and the capability people develop while delivering them. We need evidence that the work fits the system and that engineers become better able to assess the next piece. For an employer relying on those juniors to take on greater responsibility, both outcomes matter.

## 5. Two pressures on the future talent pipeline

**Purpose:** Connect the delivery observation to the paper's wider question, with labour-market evidence in a supporting role.

**Topics to cover:**

- Separate two possible pressures: fewer entry opportunities, and weaker learning for junior engineers who are still employed.
- Explain how either pressure could narrow future supply, while presenting shortages as a scenario rather than a forecast.
- Consider organisational consequences: more review pressure, dependence on external hiring, concentrated knowledge and fewer people ready for wider ownership.
- Use the current sources only as context: age is not career stage; job adverts are not hires or development; one AI company's workforce is not the whole market.
- Ask who develops future senior capability if many employers expect to hire it later.

### Your draft

As leaders, we need to consider two pressures on the future engineering talent pipeline: how many people get the opportunity to enter it, and what they learn before and after they arrive. An organisation’s own use of AI is only part of that picture.

Judgement and critical thinking begin developing long before someone’s first engineering role. If students routinely use AI to supply the answers, construct the arguments and make the connections for them, they may complete assignments while missing some of the practice those assignments were meant to provide. The cheat code is available before the first day at work. My concern is that a gap in understanding could begin in education and then be carried into a workplace that makes it equally convenient to work around it.

Hiring adds a second pressure. SignalFire’s June 2026 talent report put new-graduate and entry-level hiring approximately 65% below 2019 levels at its tracked large technology companies, and 76% below 2019 at its tracked early-stage startups. Yet engineering hiring across all experience levels at those startups was 7% above 2019. That distinction matters: recruitment can recover overall while opportunities at the beginning of a career remain constrained. These figures come from SignalFire’s proprietary profile data and selected company groups, not a census of junior software jobs. [SignalFire, 2026](https://www.signalfire.com/blog/signalfire-state-of-talent-report-2026)

Anthropic offers a more recent, narrower example. In its October 2026 report, the company said the proportion of junior technical hires had fallen from 27% to 9% since early 2025. Its total technical hiring continued to grow, so the falling share does not by itself establish a fall in the number of juniors hired. It is also one employer with an unusual relationship to AI. [Anthropic, 2026](https://www.anthropic.com/research/employment-at-anthropic)

Neither report establishes how much of the change was caused by AI. But they make the entry route worth examining alongside the learning experience. If fewer people get a first opportunity, and some of those who do receive less practice in exercising judgement, the pressures could compound. Improving one employer’s tooling would address only part of that problem.

In the short term, experienced colleagues may absorb the extra review and difficult decisions. That can make the consequences easy to miss. Over time, a narrower intake and weaker development could leave fewer engineers ready to take on wider responsibility. An employer might plan to hire that experience when it needs it, but that depends on someone having developed it elsewhere.

This is a possible future, not an inevitable shortage. AI capabilities will change, roles will evolve and engineers may adapt to these pressures, as engineers do. But that requires conscious effort and foresight. We need to plan for the capability we will need and make room for people to develop it.

## 6. The argument has serious alternatives

**Purpose:** Test the thesis against explanations and futures that could change the conclusion.

**Topics to cover:**

- AI may teach effectively through explanations, examples, critique and rapid exposure to a wider range of problems.
- Passive acceptance predates AI, and manually typing code never guaranteed learning.
- Weak outcomes may reflect onboarding, missing context, incentives, time pressure, review culture or unclear standards rather than the tool itself.
- Senior engineers are also susceptible to misplaced confidence and convenience.
- Future expertise may differ from today's; less routine implementation could coexist with stronger capability if the development path changes well.
- Identify disconfirming evidence: AI-supported engineers retain learning, transfer it to unfamiliar work, explain and challenge choices, and require less support over time despite doing less manual implementation.

### Your draft

The risks facing junior developers are not inevitable. AI tools and the ways we use them will change, and some of the answers to today’s problems may emerge through that development. We should take the possibilities for learning as seriously as the risks of bypassing it.

AI offers an opportunity to develop the very skills I am concerned about losing. A student can ask it to produce an answer, but they can also ask it to challenge their assumptions, explore alternatives and question the reasoning behind their own conclusions. Used this way, AI becomes a learning partner that helps them work through a problem and defend their choices. The same tool that makes it possible to bypass judgement can also help someone practise and strengthen it.

At work, that could mean using AI to trace a request through a codebase, explore why components interact as they do or compare possible improvements. AI never tires of you asking “Why?” Its explanations still need checking against the code and the decisions behind it, but the opportunity to investigate further is valuable. The same tool that can supply a solution can also help an engineer work through it.

There is evidence that carefully designed AI tutoring can improve learning. Kestin and colleagues studied 194 students in a Harvard undergraduate physics course, comparing a structured AI tutor with active classroom learning across two lessons. Students achieved greater immediate learning gains with the tutor. It used expert-prepared material and teaching guidance, so this was not a test of an unrestricted chatbot or of long-term engineering development. It nevertheless provides a serious example of AI supporting understanding. [Kestin et al., 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12179260/)

Evidence from programming also challenges the idea that code generation must undermine learning. In a controlled study of 69 novices aged 10–17, Kazemitabaar and colleagues found that access to an AI code generator improved code-authoring performance. The study did not find a statistically significant overall difference in retention-test performance one week later. That does not prove the approaches were equivalent or that longer-term harm is impossible. It shows why we should investigate the learning environment and the way the tool is used before assuming a particular outcome. [Kazemitabaar et al., 2023](https://arxiv.org/html/2302.07427v2)

Nor did uncritical acceptance begin with AI. A Stack Overflow answer could be copied into a codebase long before anyone could prompt a model to produce the same change. Fischer and colleagues’ 2017 analysis found security-related Stack Overflow snippets matching code in published Android applications, including snippets classified as insecure. That analysis could not tell us what individual developers understood or why they used the code. It does document the presence of insecure reused examples before generative AI. Reusing an answer has never guaranteed that it suits the application. [Fischer et al., 2017](https://conferences.computer.org/sp/pdfs/sp/2017/5533a121.pdf)

A poorly reasoned PR is therefore not enough to establish that AI caused the gap in judgement. The engineer may lack context, have had little useful coaching or be responding to pressure to finish. Those conditions deserve attention alongside the tool. Otherwise, we risk blaming AI for a development problem that would have needed addressing anyway.

We should also leave room for expertise itself to change. Future roles may place greater value on directing agents, evaluating their work and coordinating changes across a system. Engineers entering the profession with these tools could develop valuable capabilities through a different route from ours. We cannot assume that every skill we acquired will retain the same importance, or that familiarity with AI alone will prepare someone for wider responsibility.

If engineers using AI retain what they learn, apply it to unfamiliar work and take on greater responsibility with less support, that would challenge the concern running through this paper. We should be willing to recognise that outcome. The practical question is how to give it the best chance of happening.

## 7. Plan, understand, then implement

**Purpose:** Put James's proposed workflow at the practical centre of the paper.

**Topics to cover:**

1. The engineer works with AI to create an implementation plan.
2. Before implementation, the engineer summarises their understanding.
3. The summary applies relevant standards, practices and SOLID principles, explaining where logic belongs and why those choices suit the problem.
4. Implementation follows the summary. Who reviews it beforehand, and for which work, remains a design question.

- If AI produces both plan and summary, passive acceptance moves up a stage. The engineer can demonstrate their reasoning by applying standards to repo-specific choices, explaining rejected alternatives and responding to challenge.
- Keep the process proportional to novelty, risk and demonstrated capability.
- Acknowledge James's trade-off: a slower first step, with later delivery and learning gains still to evaluate.
- Use the controller example to show what should surface before code exists, without inventing code.

### Your draft

<!-- Write your paragraphs here. -->

## 8. Embed the workflow around people

**Purpose:** Show how the proposed practice becomes development rather than another compliance step.

**Topics to cover:**

- Pair explanations with mentoring, gradually increasing independence and feedback tied to real work.
- Turn review feedback into an expectation for the next comparable task, then revisit it later to test durable learning.
- Preserve real responsibility and consequences while matching support to the engineer and the risk.
- Protect time for explanation and coaching, and account honestly for the senior attention required.
- Connect this section to [*The Quiet Handover*](./TheQuietHandover-Oct2026.pdf): its discussion of judgement and approval, the loss of practice and apprenticeship, and a people-first approach provides a companion frame for where judgement comes from.
- Keep simulations, rotations, incident replays and a time-boxed pilot as optional ideas to explore, not a programme James has endorsed.

### Your draft

<!-- Write your paragraphs here. -->

## 9. Measure the whole delivery and learning loop

**Purpose:** Define how leaders could tell whether the workflow improves delivery and develops capability.

**Topics to cover:**

- Measure end-to-end delivery, including review cycles, architectural corrections and rework, rather than generation time alone.
- Look for retention and transfer: can the engineer apply prior feedback to a later or unfamiliar task?
- Assess whether the engineer can explain placement and trade-offs, challenge an AI suggestion, recognise uncertainty and take broader ownership.
- Track the support required over time, mentor workload and whether coaching remains sustainable.
- Compare outcomes by task type, risk and experience; avoid treating commits, code volume or AI usage as capability measures.
- Do not promise preserved throughput or proven learning gains. Define what evidence would justify continuing, changing or stopping the approach.

### Your draft

<!-- Write your paragraphs here. -->

## 10. Who becomes senior?

**Purpose:** Return to the title's question and leave leaders with a decision about how capability will be built.

**Topics to cover:**

- Reconnect today's review and rework cost with the longer-term question of judgement.
- State only what James's observations support, then distinguish the causal hypothesis and future risk.
- Present the workflow as a response worth testing, not a proven solution.
- Close on leadership responsibility for making reasoning, feedback, practice and growing ownership part of AI-assisted delivery.
- Leave the final line for James; do not preselect a slogan.

### Your draft

<!-- Write your concluding paragraphs here. -->

## Research and drafting notes

### Claim-status discipline

- **Observed events:** Some reviewed pull requests showed good local code but poor application fit; feedback sometimes produced short-lived improvement; James observes different questioning behaviour among many senior engineers.
- **Proposed causal explanation:** Convenience and passive acceptance may reduce the repeated critical-thinking practice through which judgement develops.
- **Future risk:** Fewer entry roles and weaker learning among employed juniors could combine to narrow the senior pipeline.
- **Proposed response:** Test plan → understanding summary → implementation inside a supported, people-first development approach.
- **Unresolved research:** Causation, prevalence, learning retention, transfer, productivity across the full delivery loop and sustainable coaching cost.

### Existing sources to retain and verify before publication

- [Stanford HAI — 2026 AI Index: Economy](https://hai.stanford.edu/ai-index/2026-ai-index-report/economy). Current outline note: reports a nearly 20% employment decline from 2024 for software developers aged 22–25, with growth in most older cohorts. **Source-review status:** underlying dataset, dates, geography and comparison groups still need review.
- [PwC — 2026 Global AI Jobs Barometer announcement, 15 June 2026](https://www.pwc.com/gx/en/news-room/press-releases/2026/pwc-2026-ai-jobs-barometer.html). Current outline note: reports that highly AI-exposed US entry-level roles are seven times more likely to request skills associated with senior work, while those “seniorised” openings also grew. **Source-review status:** full methodology and comparator still need review.
- [Anthropic — How employment is changing at Anthropic, 16 October 2026](https://www.anthropic.com/research/employment-at-anthropic). Junior technical hiring-share evidence is now used in section 5; see the reviewed source note below. Agent-runtime comparisons are not used as measures of useful output or displacement.
- [*The Quiet Handover*](./TheQuietHandover-Oct2026.pdf). Local companion paper for the established themes of judgement and approval, practice loss and apprenticeship, and people-first implementation. **Source-review status:** relevant sections identified locally; quote and page-level citation checks remain for drafting.

### Evidence used in sections 2 and 3

- [Google — What to look for in a code review](https://google.github.io/eng-practices/review/reviewer/looking-for.html). **Type:** published engineering guidance. **Supports:** separate attention to design, integration, functionality, complexity and tests. **Review:** primary guidance read. It is not experimental evidence or a mandate for our controller architecture.
- [Parnas — On the criteria to be used in decomposing systems into modules (1972)](https://doi.org/10.1145/361598.361623). **Type:** original design analysis. **Supports:** how decomposition can localise particular changes. **Review:** publisher abstract and [university-hosted reproduction](https://www.cs.lafayette.edu/~gexia/cs301/resources/parnas.html) read; the reproduction warns of possible transcription errors, so verify original wording before quoting. No empirical savings estimate or AI claim is inferred.
- [Bacchelli and Bird — Expectations, Outcomes, and Challenges of Modern Code Review (2013)](https://www.microsoft.com/en-us/research/publication/expectations-outcomes-and-challenges-of-modern-code-review/). **Type:** empirical mixed-method study at Microsoft. **Supports:** change understanding, knowledge transfer and alternative solutions in review. **Review:** authors’ publication abstract checked; full methods still to review before detailed statistical claims. It predates generative AI.
- [Lee et al. — The Impact of Generative AI on Critical Thinking (2025)](https://www.microsoft.com/en-us/research/publication/the-impact-of-generative-ai-on-critical-thinking-self-reported-reductions-in-cognitive-effort-and-confidence-effects-from-a-survey-of-knowledge-workers/). **Type:** self-reported survey of 319 knowledge workers. **Supports:** associations between confidence and reported critical thinking. **Review:** primary publication summary checked. It does not establish causation, long-term skill loss, or a junior-versus-senior difference.
- [Shen and Tamkin — How AI Impacts Skill Formation (2026)](https://arxiv.org/html/2601.20245v2). **Type:** research preprint; randomised coding experiment with 52 participants. **Supports:** immediate learning outcomes when acquiring an unfamiliar library; observed differences between interaction patterns. **Review:** primary methods and findings checked. Interaction styles were not randomised; the study does not establish long-term decline or disproportionate harm among juniors.
- **Illustration:** the controller and later policy-change scenario explain a mechanism. They contain no measured cost, real incident or claim that a particular design always wins.
- **Still unestablished:** stronger models causing less scrutiny; repeated passive acceptance causing durable skill loss; juniors being disproportionately affected; the proposed workflow reversing those effects. Keep these as questions or conditional arguments unless further evidence supports them.

### Evidence used in section 4

- [Galesic and Bosnjak — Effects of Questionnaire Length on Participation and Indicators of Response Quality in a Web Survey (2009)](https://doi.org/10.1093/poq/nfp031). **Type:** web-survey experiment. **Review:** publisher abstract checked; question order was varied. Later responses were quicker, less detailed and more uniform on the reported indicators. An analogy for sustained engagement, not direct evidence of fatigue or poorer engineering decisions in AI workflows.
- [Buçinca, Malaya and Gajos — To Trust or to Think (2021)](https://arxiv.org/html/2102.09692). **Type:** experiment analysing 199 participants using simulated AI on a nutrition task. **Review:** primary methods and findings checked. Supports reduced overreliance from interventions encouraging deliberation and a trade-off with user preference. Not a coding, junior-versus-senior or long-term learning study.
- **Framework descriptions:** official Spec Kit clarification instructions, OpenSpec FAQ and AWS AI-DLC walkthrough checked. Workflows differ: Spec Kit currently caps its clarification session at five questions, presented individually. The draft’s concern about repeated questioning is a possible interaction failure, not a claim that each framework requires a hundred questions or prevents reflection.

### Evidence used in section 5

- [SignalFire — State of Talent Report 2026](https://www.signalfire.com/blog/signalfire-state-of-talent-report-2026), published 22 June 2026. **Review:** primary report checked. Its “new grad/entry-level” metric covers 12 named large technology companies and selected VC-backed Seed–Series B startups. Profile-based estimates; exact entry-level definition, geographic coverage and latest measurement endpoint are not clearly specified. The reported changes compare with 2019, not the preceding year, and do not isolate AI’s causal effect.
- [Anthropic — How employment is changing at Anthropic](https://www.anthropic.com/research/employment-at-anthropic), published 16 October 2026. **Review:** primary company report checked. Hiring shares are distinct from counts; total technical hiring grew. “Since early 2025” is the stated comparison; the endpoint month and precise junior-band definition are not specified in the reviewed text. Single-employer evidence, not an industry estimate or causal test.
- [Bastani et al. — Generative AI without guardrails can harm learning (2025)](https://doi.org/10.1073/pnas.2422633122). **Review:** author-hosted full paper and publisher record checked. The 17% is a relative reduction in unassisted assessment scores, not percentage points. School mathematics and immediate learning outcomes; guarded tutoring mitigated harm, without demonstrating a statistically significant unassisted learning gain. Does not establish weaker university graduates or long-term workforce effects.

### Evidence used in section 6

- [Kestin et al. — AI tutoring outperforms in-class active learning (2025)](https://pmc.ncbi.nlm.nih.gov/articles/PMC12179260/). **Review:** primary full text checked. Randomised crossover trial; 194 eligible Harvard physics students, two lessons, immediate assessments. Custom tutor used expert-prepared solutions, scaffolding and teaching guidance. Supports a particular tutoring design, not universal chatbot superiority, lower learning costs or long-term professional capability.
- [Kazemitabaar et al. — Studying the effect of AI Code Generators on Supporting Novice Learners in Introductory Programming (2023)](https://arxiv.org/html/2302.07427v2). **Review:** primary methods, results and limitations checked. Controlled study of 69 young novices in a structured introductory Python environment, with manual modification tasks and feedback. No statistically significant overall retention difference after one week; lack of significance is not evidence of equivalence or absence of longer-term effects.
- [Fischer et al. — Stack Overflow Considered Harmful? (2017)](https://conferences.computer.org/sp/pdfs/sp/2017/5533a121.pdf). **Review:** primary study checked. Static matching and security classification identify insecure examples in published Android applications. Supports a historical code-reuse concern, not a finding about individual developers’ motives, understanding, architectural judgement or the security of every use in context.
- **Not established by these sources:** AI generally replaces paid education at equivalent quality or lower total cost; today’s younger entrants are inherently prepared for future senior roles; improved models alone resolve skill formation. The draft treats access benefits and changing expertise as possibilities to assess.

### Evidence to seek

- Learning and retention studies comparing different forms of AI assistance, including evidence that challenges the thesis.
- Transfer to unfamiliar work and the ability to explain or challenge future decisions without the original prompt context.
- Full delivery outcomes: generation, review, rework, defects, maintainability and operational consequences.
- Coaching capacity, mentor burden and progression towards independent ownership.
- Junior, senior, manager and career-changer perspectives, with consent for any attributed material.

### Editorial questions to resolve during drafting

- How often does the observed pattern occur, and in which kinds of work?
- What would count as adequate evidence that an engineer understands a plan before implementation?
- Where would the workflow add useful friction, and where would it become ceremony?
- Which optional development practices deserve inclusion after the central workflow has been tested?
- What evidence would cause James to revise or abandon the proposed explanation?
