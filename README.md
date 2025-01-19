##Tool Calling Agent with Structured Output
This notebook walks through how to ensure that a tool-calling agent always returns its output in a structured format. The structured output is essential when the output needs to be passed on to downstream software or users. This is done by adding a third node that formats the response before returning it to the user.

Graph Structure
The overall graph consists of:

A model node which generates responses and calls tools.
A tool-calling node which interacts with external tools.
A response node that structures the output before sending it to the user.
Options for Structured Output
There are two main options for structuring the output:

Option 1: Bind Output as Tool
In this approach, we bind the output of the tool directly to the LLM (language model) and structure the response after the tool returns its output. The expected flow is:

The LLM first selects the action tool.
After receiving the output from the action tool, the LLM calls the response tool, which structures the output before responding to the user.
Pros:
This method only requires one LLM, reducing both cost and latency.
The output is structured with minimal overhead.
Cons:
The agent might not always choose the correct tool, leading to potential errors.
If multiple tools are called, you will need to handle this in the routing logic.
Option 2: Use a Second LLM for Structured Output
In this method, the LLM calls the tools and then a second LLM with structured output responds to the user. The flow is:

The LLM chooses between the tools node and the response node.
If the response node is chosen, a second LLM structures the output before sending it to the user.
Pros:
The method guarantees structured output.
It reduces the complexity of routing logic.
Cons:
This approach requires an additional LLM call, increasing costs and latency.
If the agent LLM is not provided with the correct output schema, it may fail to call the correct tools.
