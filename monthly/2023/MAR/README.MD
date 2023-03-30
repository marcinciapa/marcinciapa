# Readings

### Implementing Domain-Driven Design (ISBN: 978-0-13-303990-0)
Finished. But I feel a need to periodically go back to individual chapters from time to time. When there will be a need to recall them during implementation. It's an interresting book, worth recommending. But hard to me to catch up without examples. 

The DDD concepts are introduced in an accessible form, easy to understand, with reasonable examples. However, in my oppinion, going all-in into DDD, as author suggests, may be counter productive. I totally support some ideas, like bounded contexts mapping, application services explanation, architectural patterns overview and many more. I find some of the ideas profitable. But not one, nor twice I saw DDD implementations causing code more complex and complicated. Use with caution.

I want my next steps in mastering DDD be implementing these book's suggestions. I need some side projects to do it. After reading the book I find [actracker-api](https://github.com/marcinciapa/actracker-api), my first attempt to implement DDD, totally wrong. I see some fields for improvement. So I feel a need to review each ideas implemented in my projects and confronting them with approaches described in this book. 

And that's my plan for practicing DDD. Implement, review with book, understand described patterns, understand them slightly better this time, correct implementation, and again.

### Clean Coder (ISBN:  978-0-13-708107-3)
Finished the first (Prefesionalism) and second (Saying No) chapter. I'm very positive about this book. It's a game changer. The hints covered in the first chapters should be obvious to anyone wanting to be considered a professional in any area. But I know only a few really representing this kind of attitude. And those are the people to admire, to learn from, true mentors. 
It's a very accessible book, my approach is to read it fragment by fragment, short, and reflect a bit on it. It's a book to fill time otherwise wasted, in a tram or waiting room at the doctor.

Here are the most prominent outcomes of the first chapter:
- Professionals take responsibility for mistakes, as they had to pay for them from their own money, not employers
- Professionals never sacriface employer's reputation for their own reputation.
- Professionals don't harm the codebase, both its functionality and its structure
- Prefessionals understand, that it's their responsibility and cost to learn, not employers. They should spend 20h a week for learning. 
- Professionals don't avoid learning the old basics: algorithms, state machines etc.
- Professionals learn the new staff: new languages when they are developers. They don't stop coding, when they are architects. They read blogs, articles, to keep up.
- Learning with others is benefitial for professionals.
- Professional train less experienced peers. They understand that when a junior fails, it's their fault.
- Professional learn the domain they work in.
- Problems of employers are problems of professionals as well. They identify with customers' problems to better assess if the requirements they are working on solve customers' issues.
- Professionals are aware that they create increadible staff: translating electic impulses into complicated applications. But they are aware that they will fail, make error in risk calculation one day. But they are first who laugh of themselves and learn from failures. They don't put fingers at others who fail, because they know that they could be next.

And the second chapter:
- There might be situation when you are affraid to say "no" to the pushy manager. In the best case you say "I'll try", which is usually a lie to avoid confrontation with attitude for failure, or admition that there are still some spare forces not demonstrated every day.
- Avoid confrontation is not a solution. You both have your objectives. The solution is to achieve a compromise: a solution with missing functionalities which can be delivered on time.
- It's not that important to explain why, technical implementation details. The exception is when manager has technical background or is ambitious enough to be interersted in your work. But it creates an opportunity for micro management, when he tells you how to do your job, what compromises you should take.
- It's event more important to say no if the stake is high. If the life of the company depends of your estimate, they should be precise and early to communicate to customers. "Firing me won't change estimates".
- It's a team sport. You are not allowed to communicate commitment, or make decision, which the team doesn't feel comfortable with.
- If the manager refuses to communicate your stong "no" up, accepting it and collecting memos is passive aggression, allowing him to hang himself up. You should be responsible for communicate up instead.
- Sometimes saying the correct "Yes" means not being affraid of saying "no".
- Those who make themselves heros should blame themselves for failures. Those are people, who think that sacrificing profesionalism and discipline may solve problem. It creates problems instead.


# Projects
### actracker-search-feeder
I created a new component in the Actracker stack: [actracker-search-feeder](https://github.com/marcinciapa/actracker-search-feeder). Feeder consumes notification messages about entities changes from RabbitMQ and puts them into ElasticSearch cluster. It is conected to it's own database to hold information about the latest version of each entity. Well, it will be. For now the database is an in-memory HashMap and the ElasticSearch cluster is simulated by printing the entity to console. But the asynchronous messages are flying around and are correctly consmed. Hopefully.

As a build tool I used Maven this time, as I don't know it too well. I went strictly the Spring Boot way this time, including involving generated, slightly changed POM from [Spring Initializr](https://start.spring.io/).

This project is a playground for practicing DDD. After the first iteration of applying it, I want to work on improving the initial DDD shape of the codebase.

The skeleton of application was [created](https://github.com/marcinciapa/actracker-search-feeder/pull/10) according to Implementing Domain Driven Design, after finishing the book. This was the first iteration of the DDD implementation, with the intention of refactoring it after going back to applicable chapters of the book:
- [Modules](https://github.com/marcinciapa/actracker-search-feeder/pull/11) chapter
- Integrating bounded contexts chapter ([notification publishing change](https://github.com/marcinciapa/actracker-api/pull/33), [notification consuming change](https://github.com/marcinciapa/actracker-search-feeder/pull/17))
- Services chapter (no changes required)
- [Application chapter](https://github.com/marcinciapa/actracker-search-feeder/pull/13)
- Architecture chapter (no changes required, but there is a potential of applying Event Sourcing)

### ACTracker
Delete activity feature added:
- https://github.com/marcinciapa/actracker-ui/pull/7
- https://github.com/marcinciapa/actracker-api/pull/32
- https://github.com/marcinciapa/actracker-search-feeder/pull/15
