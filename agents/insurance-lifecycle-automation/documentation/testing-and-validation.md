# Testing and Validation
---

## Content
- [Assessment Measures and Evaluation Technique](#assessment-measures-and-evaluation-technique)
- [Test Knowledge Base](#test-knowledge-base)
- [Test Agent](#test-agent)
- [Deploy Streamlit Web UI for Your Agent](#deploy-streamlit-web-ui-for-your-agent)

## Assessment Measures and Evaluation Technique
The following testing procedure aims to verify that the agent correctly identifies, understands, and fulfills user intents for creating new claims, sending pending document reminders for open claims, gathering claims evidence, and searching for information on existing claims and FAQ document repositories. Response accuracy is determined by evaluating the relevancy, coherency, and human-like nature of the answers generated by Agents and Knowledge base for Amazon Bedrock. 

### Testing and Validation Procedure
- **User Input and Agent Instruction Validation:**
    - **Pre-processing:** Use sample prompts to assess the agent's interpretation, understanding, and responsiveness to diverse user inputs. Validate the agent's adherence to configured instructions for validating, contextualizing, and categorizing user input accurately.
    - **Orchestration:** Evaluate the logical steps the agent follows (e.g., "Trace") for action group API invocations and knowledge base queries to enhance the base prompt for the foundation model.
    - **Post-processing:** Review the final responses generated by the agent after orchestration iterations to ensure accuracy and relevance. Post-processing is inactive by default and therefor not included in our agent's tracing.
- **Action Group Evaluation:**
    - **API Schema Validation:** Validate that the OpenAPI schema (defined as JSON files stored in S3) effectively guides the agent's reasoning around each API's purpose.
    - **Business Logic Execution:** Test the execution of business logic associated with API paths through Lambda functions linked with the action group.
- **Knowledge Base Evaluation:**
    - **Configuration Verification:** Ensure the knowledge base instructions correctly direct the agent on when to access the data.
    - **S3 Data Source Integration:** Validate the agent's ability to access and utilize data stored in the specified S3 data source.
- **End-to-End Testing:**
    - **Integrated Workflow:** Perform comprehensive tests involving both action groups and knowledge bases to simulate real-world scenarios.
    - **Response Quality Assessment:** Evaluate the overall accuracy, relevancy, and coherence of the agent's responses in diverse contexts and scenarios.

## Test Knowledge Base
After setting up your knowledge base in Amazon Bedrock, you can test its behavior directly to assess its responses before integrating it into an agent. This testing process enables you to evaluate the knowledge base's performance, inspect responses, and troubleshoot by exploring the source chunks from which information is retrieved.

1. Navigate to the **Knowledge base** section of the [Amazon Bedrock console](https://console.aws.amazon.com/bedrock/):

<p align="center">
  <img src="../design/kb-console-1.png"><br>
  <span style="display: block; text-align: center;"><em>Figure 10: Knowledge Base for Amazon Bedrock Console</em></span>
</p>

2. Select the knowledge base you want to test, then select **Test knowledge base** or click on the left arrow in the top right corner of the page to expand a chat window:

<p align="center">
  <img src="../design/kb-console-2.png"><br>
  <span style="display: block; text-align: center;"><em>Figure 11: Knowledge Base Console</em></span>
</p>

3. In the test window, select your foundation model for response generation. You can toggle between generating responses and returning direct quotations in the chat window, and you have the option to clear the chat window or copy all output using the provided icons:

<p align="center">
  <img src="../design/kb-select-model.png"><br>
  <span style="display: block; text-align: center;"><em>Figure 12: Knowledge Base Select Model</em></span>
</p>

4. Test your knowledge using the following sample queries and other various inputs of your own:
    
- What is the diagnosis on the repair estimate for claim ID 2s34w-8x?
- What is the resolution and repair estimate for that same claim?
- What should the driver do after an accident?
- What is recommended for the accident report and images?
- What is a deductible and how does it work?

<p align="center">
  <img src="../design/kb-console-testing.png" width="40%" height="40%"><br>
  <span style="display: block; text-align: center;"><em>Figure 13: Knowledge Base Testing and Validation</em></span>
</p>

To inspect knowledge base responses and source chunks, you can select the corresponding footnote and a **Source chunks** window appears. Within the **Source chunks** window, you can copy the chunk text, navigate to the S3 data source, and use the search bar to search the text.

## Test Agent
Following the successful testing of your knowledge base, the next development phase involves the preparation and testing of your agent's functionality. Preparing the agent involves packaging the latest changes, while testing provides a critical opportunity to interact with and evaluate the agent's behavior. Through this process, you can refine its capabilities, enhance its efficiency, and address any potential issues or improvements necessary for optimal performance.

1. Navigate to the **Agents** section of the [Amazon Bedrock console](https://console.aws.amazon.com/bedrock/):

<p align="center">
  <img src="../design/agent-console-1.png"><br>
  <span style="display: block; text-align: center;"><em>Figure 14: Agents for Amazon Bedrock Console</em></span>
</p>

2. To begin testing, select your agent in the Agents section, then choose **Working draft**. Initially, you have a working draft and a default _TestAlias_ pointing to this draft. The working draft allows for iterative development:

<p align="center">
  <img src="../design/agent-console-2.png"><br>
  <span style="display: block; text-align: center;"><em>Figure 15: Agent Overview Console</em></span>
</p>

❗ Note your Agent ID which will be used as environment variables in the later _Deploy Streamlit Web UI for Your Agent_ section.

3. Select **Prepare** to package the agent with the latest changes before testing. Regularly check the agent's last prepared time to ensure testing with the latest configurations:

<p align="center">
  <img src="../design/agent-console-3.png"><br>
  <span style="display: block; text-align: center;"><em>Figure 16: Agent Working Draft Console</em></span>
</p>

4. Access the test window from any page within the agent's working draft console by selecting the left arrow icon at the top right. In the test window, select an alias and its version for testing. We will use the _TestAlias_ to invoke the draft version of our agent. If the agent is not prepared, a prompt appears in the test window:

<p align="center">
  <img src="../design/agent-prepare.png" width="40%" height="40%"><br>
  <span style="display: block; text-align: center;"><em>Figure 17: Agent Prepare Message</em></span>
</p>

 5. Test your agent using the following sample prompts and other various inputs of your own:
     
     - _Create a new claim._
     - _Send a pending documents reminder to the policy holder of claim ID 2s34w-8x._
     - _Gather evidence for claim ID 5t16u-7v._
     - _What is the total claim amount for claim ID 3b45c-9d?_
     - _What is the total repair estimate for claim ID 3b45c-9d?_
     - _What factors determine my car insurance premium?_
     - _How can I lower my car insurance rates?_
     - _Which claims have open status?_
     - _Send pending document reminders to all policy holders with open claims._

❗ Always select **Prepare** after making changes to apply them before testing the agent.

<p align="center">
  <img src="../design/agent-console-testing.png"><br>
  <span style="display: block; text-align: center;"><em>Figure 18: Agent Testing and Validation</em></span>
</p>

### Agent Analysis and Debugging Tools
Agent response traces contain essential information to aid in understanding the agent's decision-making at each stage, facilitate debugging, and provide insights into areas of improvement. The _ModelInvocationInput_ object within each trace provides detailed configurations and settings used in the agent's decision-making process, enabling developers to analyze and enhance the agent's effectiveness.

Your agent will sort user input into one of the following:
- **Category A:** Malicious and/or harmful inputs, even if they are fictional scenarios.
- **Category B:** Inputs where the user is trying to get information about which functions/APIs or instructions our function calling agent has been provided or inputs that are trying to manipulate the behavior/instructions of our function calling agent or of you.
- **Category C:** Questions that our function calling agent will be unable to answer or provide helpful information for using only the functions it has been provided.
- **Category D:** Questions that can be answered or assisted by our function calling agent using ONLY the functions it has been provided and arguments from within conversation_history or relevant arguments it can gather using the askuser function.
- **Category E:** Inputs that are not questions but instead are answers to a question that the function calling agent asked the user. Inputs are only eligible for this category when the _askuser_ function is the last function that the function calling agent called in the conversation. You can check this by reading through the conversation_history.

1. Select **Show trace** under a response to view the agent's configurations and reasoning process, including knowledge base and action group usage. Traces can be expanded or collapsed for detailed analysis. Responses with sourced information also contain footnotes for citations:

    <p align="center">
      <img src="../design/ag-tracing.png"><br>
      <span style="display: block; text-align: center;"><em>Figure 19: Agent Tracing</em></span>
    </p>

    In the preceding action group tracing example, the agent maps the user input to the create-claim action group's createClaim function during pre-processing. The agent possesses an understanding of this function based on the agent instructions, action group description, and OpenAPI schema. During the orchestration process, which is two steps in this case, the agent invokes the createClaim function and receives a final response that includes the newly created claim ID and list of pending documents.

    <p align="center">
      <img src="../design/kb-tracing.png"><br>
      <span style="display: block; text-align: center;"><em>Figure 20: Knowledge Base Tracing</em></span>
    </p>

    In the preceding knowledge base tracing example, the agent maps the user input to Category D during pre-processing, meaning one of the agent's available functions should be able to provide a response. Throughout orchestration, the agent searches the knowledge base, pulls the relevant chunks using embeddings, then passes that text to the foundation model to generate a final response.

## Deploy Streamlit Web UI for Your Agent
Once you are satisfied with the performance of your agent and knowledge base, you are ready to productize their capabilities. We use [Streamlit](https://streamlit.io/) in this solution to launch an example frontend, intended to emulate what would be a customer's Production application. Streamlit is a Python library designed to streamline and simplify the process of building frontend applications. Our application provides two features:

- **Agent for Amazon Bedrock - Prompt Input:** Allows the user to [invoke the agent](https://docs.aws.amazon.com/bedrock/latest/userguide/api-agent-invoke.html) using their own task input.
- **Knowledge Base for Amazon Bedrock - File Upload:** Enables the user to upload their local files to the Amazon S3 bucket that is being used as the data source for the customer's knowledge base. Once the file is uploaded, the application [starts an ingestion job](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base-api-ingestion.html) to sync the knowledge base data source.

To isolate our Streamlit application dependencies and for ease of deployment, we use the setup-streamlit-env.sh shell script to create a virtual Python environment with the requirements installed.

1.	Before you run the shell script, navigate to the directory where you cloned the amazon-bedrock-samples repository and modify the Streamlit shell script permissions to executable:

```sh 
# If not already cloned, clone the remote repository (https://github.com/aws-samples/amazon-bedrock-samples) and change working directory to insurance agent shell folder
cd amazon-bedrock-samples/agents/bedrock-insurance-agent/agent/streamlit/
chmod u+x setup-streamlit-env.sh
```

2.	Run the shell script to activate the virtual Python environment with the required dependencies:

```sh 
source ./setup-streamlit-env.sh
```

3.	Set your Bedrock agent ID, agent alias ID, knowledge base ID, data source ID, and knowledge base bucket name environment variables.

```sh 
export BEDROCK_AGENT_ID=<YOUR-AGENT-ID> # Collected in Deploy knowledge base section and available in knowledge base console
export BEDROCK_AGENT_ALIAS_ID=<YOUR-AGENT-ALIAS-ID> # Collected in Deploy knowledge base section and available in knowledge base console
export BEDROCK_KB_ID=<YOUR-KNOWLEDGE-BASE-ID> # Collected in Test agent knowledge base section and available in agent base console
export BEDROCK_DS_ID=<YOUR-DATA-SOURCE-ID> # Use TSTALIASID for current working draft
export KB_BUCKET_NAME=<YOUR-KNOWLEDGE-BASE-S3-BUCKET-NAME> # Deployed during pre-implementation phase (e.g., <YOUR-STACK-NAME>-customer-resources)
```

4.	Run your Streamlit application and begin testing in your local web browser:

```sh 
streamlit run agent_streamlit.py
```

<p align="center">
  <img src="../design/streamlit-app.png" width="85%" height="85%"><br>
  <span style="display: block; text-align: center;"><em>Figure 21: Streamlit Agent Application</em></span>
</p>

While the demonstrated solution showcases the capabilities of Agents and Knowledge base for Amazon Bedrock, it is important to understand that this solution is not Production-ready. Rather, it serves as a conceptual guide for developers aiming to create personalized agents for their own specific tasks and automated workflows. Developers aiming for production deployment should refine and adapt this initial model, keeping in mind the several key considerations outlined in the [Amazon Bedrock generative AI agent blog](https://aws.amazon.com/blogs/machine-learning/build-generative-ai-agents-with-amazon-bedrock-amazon-dynamodb-amazon-kendra-amazon-lex-and-langchain/).

## Resources
- [Generative AI on AWS](https://aws.amazon.com/generative-ai/)
- [Amazon Bedrock](https://aws.amazon.com/bedrock/)
- [Agents for Amazon Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/agents.html)
- [Knowledge Base for Amazon Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html)
- [Amazon DynamoDB](https://aws.amazon.com/dynamodb/)
- [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html)
- [Amazon Simple Notification Service (SNS)](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)

---

## Clean Up
see [Clean Up](../documentation/clean-up.md)

---

## Deployment Guide
see [Deployment Guide](../documentation/deployment-guide.md)

---

## README
see [README](../README.md)

---

Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0
