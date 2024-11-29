# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 1000 (from the original 50000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Amina Choua

Please write your explanation here...

## The Sampling Procedure

The sampling procedure occurs at several stages within the `simulate_event()` function, as detailed below:

### 1. Infection Sampling
A random subset of attendees at events is sampled to simulate infections.
- **Function used:** `np.random.choice()`
- **Sample size:** 10% of the total event attendees, determined by the variable `ATTACK_RATE`
- **Sampling frame:** All attendees
- **Distribution:** Uniform, with each attendee having an equal chance of infection
- **Blog:** The model infects a fixed percentage (10%) across events, as seen in the blog. This assumes a homogeneous infection rate for simplicity, though actual exposure rates may differ depending on event settings.

### 2. Primary Contact Tracing Sampling
Applied to infected individuals to determine which ones are successfully traced.
- **Function used:** `np.random.rand()`
- **Sample size:** 20% of infected individuals
- **Sampling frame:** Infected individuals only
- **Distribution:** This stage uses a Bernoulli distribution with a probability parameter `TRACE_SUCCESS`
- **Blog:** The model assigns a 20% chance of successful tracing for infected individuals, reflecting the blog's note that only a small portion of cases are traceable, often influenced by available resources and recall limitations.

### 3. Secondary Contact Tracing Sampling
Traces infected attendees at events with 2+ primary traces.
- **Function used:** `event_trace_counts[event_trace_counts >= SECONDARY_TRACE_THRESHOLD].index`
- **Sample size:** Depends on the number of traced infections per event and how many attendees were infected
- **Sampling frame:** All attendees of each event type (wedding and brunch), but only for those events where primary tracing identified multiple cases
- **Distribution:** This secondary sampling does not follow a specific statistical distribution but instead uses conditional logic based on the threshold of traced cases per event type
- **Blog:** When an event meets a threshold of traced cases, it triggers more extensive tracing, as assumed in the blog’s model. This prioritizes larger or easier-to-trace events, like weddings, which can result in overrepresenting these events in traced cases, leading to bias.

---

## Comparison between the Graphs in the Blog and the Code

In the blog post, the two key graphs separately show the true proportion of infections from weddings and the observed (traced) proportion, highlighting the bias created by tracing. In contrast, this code combines infections and traced cases in a single plot, making it harder to isolate and directly visualize the bias effect.

---

## Observations with 1000 Iterations instead of 50,000

When running the script multiple times using 1000 iterations, I observe that the shapes and peaks of the histograms vary more between runs. This makes the results less reproducible because each run can yield notably different distributions of the proportions of infections and traces attributed to weddings, depending on random sampling variability.

---

## Reproducibility

To improve the reproducibility of the script, I introduced a random seed by adding the line `np.random.seed(42)` at the beginning of the code. Each time the script is executed with this seed in place, it will produce identical distributions and graphs, allowing for consistent comparison between runs.



## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `HH:MM AM/PM - DD/MM/YYYY`
* The branch name for your repo should be: `sampling-and-reproducibility`
* What to submit for this assignment:
    * This markdown file (sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `sampling-and-reproducibility`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack at `#cohort-3-help`. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
