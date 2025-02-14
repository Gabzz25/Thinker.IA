"scripts": {
  "start": "react-scripts start",
  "build": "react-scripts build"
}

import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { MessageSquare } from "lucide-react";

export default function ThinkerIA() {
  const [input, setInput] = useState("");
  const [response, setResponse] = useState("");
  const [loading, setLoading] = useState(false);

  const handleSubmit = async () => {
    if (!input.trim()) return;
    setLoading(true);
    try {
      const res = await fetch("https://api.openai.com/v1/chat/completions", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer YOUR_OPENAI_API_KEY`,
        },
        body: JSON.stringify({
          model: "gpt-3.5-turbo",
          messages: [{ role: "user", content: input }],
        }),
      });
      const data = await res.json();
      setResponse(data.choices[0].message.content);
    } catch (error) {
      setResponse("Erro ao obter resposta. Tente novamente.");
    }
    setLoading(false);
  };

  return (
    <div className="flex flex-col items-center p-6 min-h-screen bg-gray-100">
      <h1 className="text-3xl font-bold mb-4">Thinker IA</h1>
      <Card className="w-full max-w-md p-4">
        <CardContent>
          <Input
            placeholder="Faça uma pergunta..."
            value={input}
            onChange={(e) => setInput(e.target.value)}
            className="mb-4"
          />
          <Button onClick={handleSubmit} disabled={loading}>
            {loading ? "Pensando..." : "Perguntar"}
          </Button>
          {response && (
            <div className="mt-4 p-3 border rounded bg-white shadow-sm">
              <MessageSquare className="inline-block mr-2 text-gray-600" />
              {response}
            </div>
          )}
        </CardContent>
      </Card>
    </div>
  );
}
