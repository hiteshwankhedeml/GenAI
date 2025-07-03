# CrewAI - When to use Crew vs Flow

| Use Case                | Recommended Approach                                        | Why?                                                                                                                                                                                      |
| ----------------------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Open-ended research** | [Crews](https://docs.crewai.com/en/guides/crews/first-crew) | When tasks require creative thinking, exploration, and adaptation                                                                                                                         |
| **Content generation**  | [Crews](https://docs.crewai.com/en/guides/crews/first-crew) | For collaborative creation of articles, reports, or marketing materials                                                                                                                   |
| **Decision workflows**  | [Flows](https://docs.crewai.com/en/guides/flows/first-flow) | When you need predictable, auditable decision paths with precise control                                                                                                                  |
| **API orchestration**   | [Flows](https://docs.crewai.com/en/guides/flows/first-flow) | For reliable integration with multiple external services in a specific sequence                                                                                                           |
| **Hybrid applications** | Combined approach                                           | Use [Flows](https://docs.crewai.com/en/guides/flows/first-flow) to orchestrate overall process with [Crews](https://docs.crewai.com/en/guides/crews/first-crew) handling complex subtasks |

#### [​](https://docs.crewai.com/en/introduction#decision-framework)Decision Framework <a href="#decision-framework" id="decision-framework"></a>

* **Choose** [**Crews**](https://docs.crewai.com/en/guides/crews/first-crew) **when:** You need autonomous problem-solving, creative collaboration, or exploratory tasks
* **Choose** [**Flows**](https://docs.crewai.com/en/guides/flows/first-flow) **when:** You require deterministic outcomes, auditability, or precise control over execution
* **Combine both when:** Your application needs both structured processes and pockets of autonomous intelligence
