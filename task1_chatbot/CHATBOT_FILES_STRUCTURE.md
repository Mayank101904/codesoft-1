
---

## 🔧 Files to Create/Update

### **1. client/components/ChatBot.tsx** ⭐ NEW FILE
**Location:** `client/components/ChatBot.tsx`

```tsx
import { useState, useRef, useEffect } from "react";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Send, MessageCircle } from "lucide-react";
import { cn } from "@/lib/utils";

interface Message {
  id: string;
  text: string;
  sender: "user" | "bot";
  timestamp: Date;
}

const chatbotRules = [
  {
    patterns: [
      /hello|hi|hey|greetings?/i,
      /what's up|sup|howdy/i,
    ],
    responses: [
      "Hey there! 👋 Welcome to our chatbot. How can I help you today?",
      "Hi! Great to see you. What can I assist you with?",
      "Hello! I'm here to help. What do you need?",
    ],
  },
  {
    patterns: [
      /how are you|how's it going|how do you feel/i,
      /what's new|what's happening/i,
    ],
    responses: [
      "I'm doing great, thanks for asking! Ready to help with anything.",
      "All systems operational! How can I assist you?",
      "I'm functioning perfectly! What brings you here?",
    ],
  },
  {
    patterns: [
      /what can you do|what are your capabilities|what features|help/i,
      /tell me what you can do/i,
    ],
    responses: [
      "I'm a rule-based chatbot that can:\n• Answer greeting messages\n• Help with common questions\n• Provide information\n• Have a friendly conversation\nJust ask me anything!",
      "I can help you with:\n• General questions\n• Friendly conversation\n• Basic information\nTry asking me something!",
    ],
  },
  {
    patterns: [
      /thank you|thanks|thank you so much|appreciate it/i,
      /thx|ty/i,
    ],
    responses: [
      "You're welcome! Happy to help! 😊",
      "My pleasure! Feel free to ask anytime.",
      "Glad I could assist! Anything else?",
    ],
  },
  {
    patterns: [
      /bye|goodbye|see you|farewell|see you later/i,
      /talk to you later|ttyl|gotta go/i,
    ],
    responses: [
      "Goodbye! Have a great day! 👋",
      "See you later! Thanks for chatting!",
      "Catch you soon! Take care!",
    ],
  },
  {
    patterns: [
      /what's your name|who are you|what do i call you/i,
      /introduce yourself/i,
    ],
    responses: [
      "I'm a rule-based chatbot, ready to assist! You can just call me ChatBot. 🤖",
      "I'm your friendly ChatBot assistant!",
      "You can call me ChatBot! Nice to meet you!",
    ],
  },
  {
    patterns: [
      /how does this work|how do you work|how are you built/i,
      /explain yourself/i,
    ],
    responses: [
      "I use rule-based pattern matching to understand your messages and provide relevant responses. I'm designed to recognize keywords and phrases to give you helpful answers!",
      "I work by analyzing patterns in your text and matching them against predefined rules to generate appropriate responses.",
      "I'm built using if-else statements and pattern matching to understand what you're asking!",
    ],
  },
  {
    patterns: [
      /joke|tell me something funny|make me laugh/i,
      /something funny/i,
    ],
    responses: [
      "Why don't scientists trust atoms? Because they make up everything! 😄",
      "I told my computer I needed a break, and now it won't stop sending me Kit-Kat ads! 🤣",
      "Why did the AI go to school? To improve its learning algorithms! 🎓",
    ],
  },
];

function getBotResponse(userMessage: string): string {
  for (const rule of chatbotRules) {
    for (const pattern of rule.patterns) {
      if (pattern.test(userMessage)) {
        return rule.responses[
          Math.floor(Math.random() * rule.responses.length)
        ];
      }
    }
  }

  const defaultResponses = [
    "That's interesting! I'm still learning about that topic. Could you rephrase your question?",
    "I'm not quite sure about that. Try asking me something else!",
    "Hmm, that's a bit beyond my current knowledge. Do you have another question?",
    "I don't have a specific response for that, but I'm always learning! What else can I help with?",
  ];

  return defaultResponses[Math.floor(Math.random() * defaultResponses.length)];
}

export default function ChatBot() {
  const [messages, setMessages] = useState<Message[]>([
    {
      id: "1",
      text: "Hi there! 👋 I'm a rule-based chatbot. You can ask me anything, and I'll do my best to help. Try saying hello!",
      sender: "bot",
      timestamp: new Date(),
    },
  ]);
  const [input, setInput] = useState("");
  const [isLoading, setIsLoading] = useState(false);
  const messagesEndRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    messagesEndRef.current?.scrollIntoView({ behavior: "smooth" });
  }, [messages]);

  const handleSendMessage = () => {
    if (!input.trim()) return;

    const userMessage: Message = {
      id: Date.now().toString(),
      text: input,
      sender: "user",
      timestamp: new Date(),
    };

    setMessages((prev) => [...prev, userMessage]);
    setInput("");
    setIsLoading(true);

    setTimeout(() => {
      const botResponse: Message = {
        id: (Date.now() + 1).toString(),
        text: getBotResponse(input),
        sender: "bot",
        timestamp: new Date(),
      };
      setMessages((prev) => [...prev, botResponse]);
      setIsLoading(false);
    }, 300);
  };

  const handleKeyPress = (e: React.KeyboardEvent) => {
    if (e.key === "Enter" && !e.shiftKey) {
      e.preventDefault();
      handleSendMessage();
    }
  };

  return (
    <div className="flex flex-col h-screen bg-gradient-to-br from-blue-50 via-white to-indigo-50">
      <div className="bg-gradient-to-r from-blue-500 to-indigo-600 text-white px-6 py-4 shadow-lg">
        <div className="flex items-center gap-3">
          <div className="bg-white/20 p-2 rounded-lg">
            <MessageCircle className="w-6 h-6" />
          </div>
          <div>
            <h1 className="text-2xl font-bold">ChatBot</h1>
            <p className="text-blue-100 text-sm">Rule-Based Responses</p>
          </div>
        </div>
      </div>

      <div className="flex-1 overflow-y-auto px-6 py-6 space-y-4">
        {messages.map((message) => (
          <div
            key={message.id}
            className={cn(
              "flex gap-3",
              message.sender === "user" ? "justify-end" : "justify-start"
            )}
          >
            {message.sender === "bot" && (
              <div className="flex-shrink-0 w-8 h-8 rounded-full bg-gradient-to-br from-blue-400 to-indigo-500 flex items-center justify-center text-white text-sm font-bold">
                CB
              </div>
            )}
            <div
              className={cn(
                "max-w-xs lg:max-w-md xl:max-w-lg px-4 py-3 rounded-lg break-words",
                message.sender === "user"
                  ? "bg-blue-500 text-white rounded-br-none"
                  : "bg-white text-gray-800 shadow-md border border-gray-100 rounded-bl-none"
              )}
            >
              <p className="whitespace-pre-line">{message.text}</p>
              <p
                className={cn(
                  "text-xs mt-1",
                  message.sender === "user"
                    ? "text-blue-100"
                    : "text-gray-500"
                )}
              >
                {message.timestamp.toLocaleTimeString([], {
                  hour: "2-digit",
                  minute: "2-digit",
                })}
              </p>
            </div>
            {message.sender === "user" && (
              <div className="flex-shrink-0 w-8 h-8 rounded-full bg-gradient-to-br from-gray-600 to-gray-800 flex items-center justify-center text-white text-sm font-bold">
                You
              </div>
            )}
          </div>
        ))}
        {isLoading && (
          <div className="flex gap-3 justify-start">
            <div className="flex-shrink-0 w-8 h-8 rounded-full bg-gradient-to-br from-blue-400 to-indigo-500 flex items-center justify-center text-white text-sm font-bold">
              CB
            </div>
            <div className="bg-white text-gray-800 shadow-md border border-gray-100 px-4 py-3 rounded-lg rounded-bl-none flex gap-2">
              <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce"></div>
              <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce" style={{ animationDelay: "0.1s" }}></div>
              <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce" style={{ animationDelay: "0.2s" }}></div>
            </div>
          </div>
        )}
        <div ref={messagesEndRef} />
      </div>

      <div className="border-t border-gray-200 bg-white px-6 py-4 shadow-lg">
        <div className="flex gap-3">
          <Input
            value={input}
            onChange={(e) => setInput(e.target.value)}
            onKeyPress={handleKeyPress}
            placeholder="Type your message... (Try 'hello', 'help', or 'joke')"
            disabled={isLoading}
            className="flex-1 border-gray-300 focus:border-blue-500 focus:ring-blue-500"
          />
          <Button
            onClick={handleSendMessage}
            disabled={isLoading || !input.trim()}
            className="bg-gradient-to-r from-blue-500 to-indigo-600 hover:from-blue-600 hover:to-indigo-700 text-white px-6"
          >
            <Send className="w-4 h-4" />
          </Button>
        </div>
        <p className="text-xs text-gray-500 mt-2">
          💡 Try asking: "What can you do?", "Tell me a joke", or "Goodbye"
        </p>
      </div>
    </div>
  );
}
```

---

### **2. client/pages/Index.tsx** (UPDATE)
**Location:** `client/pages/Index.tsx`

```tsx
import ChatBot from "@/components/ChatBot";

export default function Index() {
  return <ChatBot />;
}
```

---

### **3. client/global.css** (UPDATE)
**Location:** `client/global.css`

Update the `:root` colors section to use blue/indigo theme:

```css
:root {
  --background: 0 0% 100%;
  --foreground: 222.2 84% 4.9%;

  --card: 0 0% 100%;
  --card-foreground: 222.2 84% 4.9%;

  --popover: 0 0% 100%;
  --popover-foreground: 222.2 84% 4.9%;

  --primary: 217.2 91.2% 59.8%;
  --primary-foreground: 222.2 47.4% 11.2%;

  --secondary: 210 40% 96.1%;
  --secondary-foreground: 222.2 47.4% 11.2%;

  --muted: 210 40% 96.1%;
  --muted-foreground: 215.4 16.3% 46.9%;

  --accent: 217.2 91.2% 59.8%;
  --accent-foreground: 222.2 47.4% 11.2%;

  --destructive: 0 84.2% 60.2%;
  --destructive-foreground: 210 40% 98%;

  --border: 214.3 31.8% 91.4%;
  --input: 214.3 31.8% 91.4%;
  --ring: 217.2 91.2% 59.8%;

  --radius: 0.5rem;
  /* ... rest of variables remain same ... */
}
```

---

