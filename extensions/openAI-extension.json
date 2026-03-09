// TurboWarp ChatGPT Extension
// Color: #74aa9c

class ChatGPTExtension {
  constructor() {
    this.apiKey = "";
    this.systemPrompt = "";
    this.messages = [];
    this.busy = false;
    this.lastError = "";
  }

  getInfo() {
    return {
      id: "chatgpt",
      name: "ChatGPT",
      color1: "#74aa9c",
      color2: "#74aa9c",
      color3: "#74aa9c",
      blocks: [
        {
          opcode: "setApiKey",
          blockType: Scratch.BlockType.COMMAND,
          text: "set OpenAI key to [KEY]",
          arguments: {
            KEY: { type: Scratch.ArgumentType.STRING }
          }
        },
        {
          opcode: "setSystemPrompt",
          blockType: Scratch.BlockType.COMMAND,
          text: "set system prompt to [TEXT]",
          arguments: {
            TEXT: { type: Scratch.ArgumentType.STRING }
          }
        },
        {
          opcode: "clearSystemPrompt",
          blockType: Scratch.BlockType.COMMAND,
          text: "clear system prompt"
        },
        {
          opcode: "askChatGPT",
          blockType: Scratch.BlockType.REPORTER,
          text: "ask ChatGPT [PROMPT]",
          arguments: {
            PROMPT: { type: Scratch.ArgumentType.STRING }
          }
        },
        {
          opcode: "askChatGPTBrief",
          blockType: Scratch.BlockType.REPORTER,
          text: "ask ChatGPT [PROMPT] with brief answer",
          arguments: {
            PROMPT: { type: Scratch.ArgumentType.STRING }
          }
        },
        {
          opcode: "askChatGPTLong",
          blockType: Scratch.BlockType.REPORTER,
          text: "ask ChatGPT [PROMPT] with long answer",
          arguments: {
            PROMPT: { type: Scratch.ArgumentType.STRING }
          }
        },
        {
          opcode: "isReady",
          blockType: Scratch.BlockType.BOOLEAN,
          text: "ChatGPT is ready?"
        },
        {
          opcode: "lastErrorReporter",
          blockType: Scratch.BlockType.REPORTER,
          text: "last ChatGPT error"
        },
        {
          opcode: "clearContext",
          blockType: Scratch.BlockType.COMMAND,
          text: "clear ChatGPT context"
        },
        {
          opcode: "getMessages",
          blockType: Scratch.BlockType.REPORTER,
          text: "messages"
        },
        {
          opcode: "clearMessages",
          blockType: Scratch.BlockType.COMMAND,
          text: "clear messages array"
        }
      ]
    };
  }

  setApiKey(args) {
    this.apiKey = args.KEY.trim();
  }

  setSystemPrompt(args) {
    this.systemPrompt = args.TEXT;
  }

  clearSystemPrompt() {
    this.systemPrompt = "";
  }

  isReady() {
    return Boolean(this.apiKey) && !this.busy;
  }

  lastErrorReporter() {
    return this.lastError;
  }

  clearContext() {
    this.messages = [];
  }

  getMessages() {
    return JSON.stringify(this.messages);
  }

  clearMessages() {
    this.messages = [];
  }

  async ask(prompt, mode) {
    if (!this.apiKey || this.busy) return "";

    this.busy = true;
    this.lastError = "";

    try {
      if (this.messages.length === 0 && this.systemPrompt) {
        this.messages.push({
          role: "system",
          content: this.systemPrompt
        });
      }

      this.messages.push({
        role: "user",
        content: prompt
      });

      let maxTokens = 256;
      if (mode === "brief") maxTokens = 80;
      if (mode === "long") maxTokens = 512;

      const response = await fetch("https://api.openai.com/v1/chat/completions", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Authorization": `Bearer ${this.apiKey}`
        },
        body: JSON.stringify({
          model: "gpt-4.1-mini",
          messages: this.messages,
          max_tokens: maxTokens
        })
      });

      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }

      const data = await response.json();
      const reply = data.choices[0].message.content;

      this.messages.push({
        role: "assistant",
        content: reply
      });

      return reply;
    } catch (e) {
      this.lastError = e.message;
      return "";
    } finally {
      this.busy = false;
    }
  }

  askChatGPT(args) {
    return this.ask(args.PROMPT, "normal");
  }

  askChatGPTBrief(args) {
    return this.ask(args.PROMPT, "brief");
  }

  askChatGPTLong(args) {
    return this.ask(args.PROMPT, "long");
  }
}

Scratch.extensions.register(new ChatGPTExtension());
