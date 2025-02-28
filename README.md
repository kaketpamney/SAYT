import { useState, useEffect } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Sun, Moon } from "lucide-react";

export default function NewsApp() {
  const [news, setNews] = useState([]);
  const [theme, setTheme] = useState("light");
  const [search, setSearch] = useState("");

  const API_KEY = "nMwhH46b-pjoa4TmnFWSK-HT6-36dDjukztPeuWgZhpofWtO";

  useEffect(() => {
    fetch(`https://api.currentsapi.services/v1/latest-news?apiKey=${API_KEY}`)
      .then((res) => res.json())
      .then((data) => setNews(data.news))
      .catch((error) => console.error("Ошибка загрузки новостей", error));
  }, []);

  return (
    <div className={`${theme === "dark" ? "bg-gray-900 text-white" : "bg-white text-black"} min-h-screen p-6`}> 
      <div className="flex justify-between mb-4">
        <input
          type="text"
          placeholder="Поиск новостей..."
          className="p-2 border rounded w-1/2"
          onChange={(e) => setSearch(e.target.value)}
        />
        <Button onClick={() => setTheme(theme === "dark" ? "light" : "dark")}> 
          {theme === "dark" ? <Sun /> : <Moon />} 
        </Button>
      </div>
      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
        {news.filter(n => n.title.toLowerCase().includes(search.toLowerCase())).map((article, index) => (
          <Card key={index}>
            <CardContent>
              <h2 className="text-xl font-bold">{article.title}</h2>
              <p>{article.description}</p>
            </CardContent>
          </Card>
        ))}
      </div>
    </div>
  );
}
