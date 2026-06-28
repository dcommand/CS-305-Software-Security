# CS-305-Software-Security
8-1 Journal: Portfolio Submission
Artemis Financial: Software Security Portfolio (CS 305)

Introduction

This portfolio holds my work from the Artemis Financial security project. It pairs the Vulnerability Assessment Report with the dependency scan that backs it. The report shows how I read the code, ran the tools, and planned the fixes. This reflection explains the choices behind that work. It also records what the tooling taught me, including the parts that fought back.

The Client and the Requirements

Artemis Financial is a financial planning company. It handles sensitive client data, including savings, retirement accounts, investments, and insurance. The company runs a RESTful web application built on Spring Boot and Maven. They asked me to harden that application against outside threats. I had to assess its vulnerabilities, protect data in transit and at rest, and align the system with financial rules like GLBA and SEC mandates.

What I Did Well

I ran the review in two layers. First, I read the source code by hand across seven security categories: input validation, APIs, cryptography, client and server code, code errors, code quality, and encapsulation. That pass exposed hardcoded credentials, weak encapsulation, and SQL injection risk. Second, I ran the OWASP Dependency-Check plugin against the build. The scan flagged 218 vulnerabilities across 122 unique CVEs in 13 dependencies. The two methods caught different problems, which is the point of using both.

Why Secure Coding Matters

Secure coding builds protection into the software from the start. It treats security as part of the design, not a patch bolted on later. In finance, one exploit can expose client identities or drain accounts. Strong security guards the brand, keeps customer trust, and avoids the heavy cost of cleaning up a breach. It also keeps the company on the right side of the law and its fines.

What Challenged Me and What Helped

The seven security categories helped the most. They gave me a clear rubric, so I read the code in order instead of hunting for flaws at random. The hard part was OWASP itself. Dependency-Check tried to download the entire National Vulnerability Database on every run, and the pull either crawled or hit the NVD rate limit. I burned real time waiting on that database before I traced the cause. I fixed it by disabling auto-update with the -DautoUpdate=false flag, which reused the cached database after the first build.

How I Increased the Layers of Security

I built the fixes as defense in depth. At the framework level, I planned a jump from Spring Boot 2.2.4 to the 3.2.x line. That single upgrade clears most of the vulnerable libraries. At the network level, I added HTTPS with TLS and planned Spring Security for access control. At the code level, I swapped manual queries for parameterized PreparedStatement calls, moved configuration out of the source, and tightened the encapsulation modifiers. Each layer covers a gap the others miss.

How I Will Assess Vulnerabilities Next Time

Next time I will catch problems earlier. I plan to run static analysis tools like SonarQube or Checkmarx beside the dependency checker. I will wire both into the CI/CD pipeline, so the scan fires the moment I push code. That moves security from a late report to a constant check. The auto-update lesson stays with me too, since a cached database keeps those scans fast.

How I Kept the Code Functional and Secure

I checked security through manual review and the dependency scan. I kept the code working by following solid patterns, such as the Repository Pattern, and by finishing the broken showInfo() method that had stopped the build. To guard against regressions, I reran OWASP Dependency-Check after the refactor, so a POM upgrade could not slip in fresh vulnerabilities. I also leaned on JUnit 5 and Mockito tests to prove the core logic still ran while the patches landed.

Tools and Practices Worth Keeping

A few habits earned a permanent place in my workflow. I will keep OWASP Dependency-Check on every Java and Maven project, with the cache configured so it runs fast. I will track the National Vulnerability Database and the OWASP Top 10 to stay current on threats. In the code itself, I will write parameterized queries by default, lock down access modifiers, store secrets outside the source, and whitelist input. These practices stop whole classes of bugs before they start.

What I Can Show an Employer

This project gives me a full Vulnerability Assessment and Mitigation Plan to put in front of an employer. It shows that I can take a legacy, insecure codebase and harden it with both manual auditing and automated scanning. It also shows that I work through real tooling friction, like the NVD download problem, instead of giving up on it. Most of all, it proves I understand the financial and legal stakes and can plan a fix that holds.

Conclusion

The Artemis Financial project taught me to secure software in layers and to read code with a checklist, not a hunch. The OWASP tooling tested my patience, but tracing the auto-update problem taught me more than a clean run would have. I leave CS 305 able to assess a codebase, remediate it, and prove the fix still works. That is the skill set I will carry into the next project.
