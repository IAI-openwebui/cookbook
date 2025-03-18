# **Function Calling Support in Open WebUI**
---
 ![44303623](https://github.com/user-attachments/assets/0e5f8ae2-a609-4154-b565-b4bff05008f1) [Originally posted](https://github.com/open-webui/open-webui/discussions/11820) by [@rifkisarici](https://github.com/rifkisarici)
___
### **Is Function Calling Possible in Open WebUI?**
Hello community,

I have been researching how to implement function calling in Open WebUI and have gathered some findings. However, some aspects are still unclear, and I would like to hear your thoughts.

### **📌 Summary:**
* **Does Open WebUI natively support OpenAI's "Function Calling" API feature?**
* **These tools seem to allow executing custom Python scripts within Open WebUI's backend, but does this truly align with function calling?**
* **Using system prompts to directly trigger OpenAI Function Calling does not seem to work. Is there a way to achieve function calling purely through system prompts?**

## **🚀 Function Calling Simulation in Open WebUI (Using Tools)**
We were able to define and execute our own functions using the **"Tools"** system in Open WebUI. However, is this truly the same as OpenAI’s function calling?

### **🛠 Step 1: Defining a Tool**
We used the following structure to add a tool in Open WebUI:

📌 **Example tool (function) definition:**

```python
from open_webui.tools import register_tool

@register_tool
def check_system_status():
    """
    A tool to check whether the system is active.
    """
    print("✅ check_system_status() function executed!")
    return "System status: Active"
```

💡 **This function was registered as a tool in Open WebUI and could be triggered by the assistant. However, does it fully replicate OpenAI’s function calling mechanism?**

### **🛠 Step 2: Adding the Tool to Open WebUI**
1. **We added the tool file to Open WebUI’s "tools" directory.**
2. **We restarted Open WebUI.**
3. **This method allowed us to execute function calling within Open WebUI.**

However, we are uncertain whether this method constitutes **true function calling**. Does Open WebUI natively support function calling, or are we just emulating similar functionality with tools?

## **🚀 Alternative Approach: Function Calling Simulation with a Filter Class**
The following **Filter** class analyzes incoming messages and triggers function calls based on certain keywords.

```python
from typing import Optional

class Filter:
    def __init__(self):
        pass

    def check_system_status(self) -str:
        print("✅ check_system_status() function executed!")
        return "System status: Active"

    def outlet(self, body: dict, __user__: Optional[dict] = None) -dict:
        print("📢 outlet is running!")

        messages = body.get("messages", [])
        user_message = messages[-2].get("content", "") if len(messages) 1 else ""

        if "check_system_status" in user_message:
            function_result = self.check_system_status()
            print(f"✅ Function Result: {function_result}")
            body["messages"].append({"role": "assistant", "content": function_result})

        return body
```

💡 **This code allowed us to manually trigger function execution. However, it does not provide the same automatic process as OpenAI's function calling API.**

So, in Open WebUI, **should we rely on such manual solutions, or is there a more integrated approach for function calling?**

## **🚀 Conclusions & Questions**
* **Does Open WebUI natively support OpenAI’s function calling, or are we simply replicating a similar system using Tools and Functions?**
* **Is it possible to trigger function calling solely through system prompts, or is backend modification required?**
* **Are there any alternative or better approaches?**

🚀 **Does anyone have more insights on this? Any recommendations for alternative solutions?**

Thank you!

