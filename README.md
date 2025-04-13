# RoshSubs
import { BrowserRouter as Router, Routes, Route, useNavigate, useParams } from "react-router-dom";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Search, Download } from "lucide-react";

const novels = {
  "جارتي الملاك": {
    volumes: 1,
    character: "ميلا",
    eyeColor: "#f0c060",
    bgColor: "from-amber-100 to-amber-200",
    buttonColor: "bg-amber-500 hover:bg-amber-600",
    greeting: "أهلاً بك في عالم الملائكة!",
    description: "رواية رائعة عن الملائكة",
    image: "angel.jpg",
    author: "مؤلف مجهول",
    status: "مكتمل",
  },
  "روشيدير": {
    volumes: 4.5,
    character: "إيلا",
    eyeColor: "#b0b0c0",
    bgColor: "from-gray-100 to-blue-100",
    buttonColor: "bg-blue-500 hover:bg-blue-600",
    greeting: "مرحباً بك في مملكة روشيدير!",
    description: "رواية عن مملكة سحرية",
    image: "roshider.jpg",
    author: "مؤلف مجهول",
    status: "جارية",
  },
  "المحققة ميتة بالفعل": {
    volumes: 1,
    character: "يوكي",
    eyeColor: "#9370DB",
    bgColor: "from-purple-100 to-purple-200",
    buttonColor: "bg-purple-500 hover:bg-purple-600",
    greeting: "هل أنت مستعد لحل الألغاز معي؟",
    description: "رواية غامضة عن محققة",
    image: "detective.jpg",
    author: "مؤلف مجهول",
    status: "مكتمل",
  }
};

function Home() {
  const navigate = useNavigate();

  const handleLogin = () => navigate("/roshsubs/login");
  const handleSearch = () => {
    const query = prompt("أدخل اسم الرواية أو المؤلف:");
    if (query) alert(`نتائج البحث عن: ${query}`);
  };
  const handleRead = (title) => {
    navigate(`/roshsubs/novels/${title}/volumes`);
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-gray-50 to-blue-50 text-right" dir="rtl">
      <header className="bg-white shadow sticky top-0 z-20">
        <div className="container mx-auto px-4 py-4 flex justify-between items-center">
          <h1 className="text-xl font-bold text-blue-800">Roshsubs</h1>
          <div className="flex gap-2">
            <Button variant="outline" size="sm" onClick={handleSearch}><Search className="h-4 w-4 ml-1" /> بحث</Button>
            <Button size="sm" onClick={handleLogin}>تسجيل الدخول</Button>
          </div>
        </div>
      </header>

      <section className="bg-blue-900 text-white py-20 text-center">
        <h2 className="text-4xl font-bold mb-4">مرحبًا بك في عالم الروايات المترجمة</h2>
        <p className="text-lg mb-8">استمتع بقراءة أفضل الروايات اليابانية مترجمة للعربية</p>
        <Button className="bg-violet-500 hover:bg-violet-600 px-8 py-4 text-lg rounded-full" onClick={() => document.getElementById("featured-novels")?.scrollIntoView({ behavior: 'smooth' })}>ابدأ الرحلة الآن</Button>
      </section>

      <section id="featured-novels" className="container mx-auto px-4 py-12 grid gap-6 md:grid-cols-2 lg:grid-cols-3">
        {Object.entries(novels).map(([title, details]) => (
          <Card key={title} className={`shadow-md overflow-hidden bg-gradient-to-br ${details.bgColor}`}>
            <CardContent className="p-0">
              <div className="h-32 flex items-center justify-center relative">
                <p className="absolute text-center text-lg font-bold" style={{ color: details.eyeColor }}>{details.greeting}</p>
              </div>
              <div className="p-4 space-y-4">
                <h3 className="text-xl font-bold" style={{ color: details.eyeColor }}>{title}</h3>
                <p className="text-sm text-gray-600">بطولة: {details.character}</p>
                <Button className={`w-full ${details.buttonColor} text-white`} onClick={() => handleRead(title)}>قراءة مباشرة</Button>
              </div>
            </CardContent>
          </Card>
        ))}
      </section>
    </div>
  );
}

function VolumeList() {
  const { novelTitle } = useParams();
  const novel = novels[novelTitle];

  if (!novel) return <div className="p-8">الرواية غير موجودة.</div>;

  const handleDownload = (volume) => {
    alert(`جاري تحميل ${novelTitle} - المجلد ${volume}`);
  };

  return (
    <div className="min-h-screen text-right bg-gradient-to-br from-gray-50 to-blue-50 p-6" dir="rtl">
      <h2 className="text-3xl font-bold mb-6">{novelTitle}</h2>
      <img src={novel.image} alt={novelTitle} className="w-full h-64 object-cover mb-6" />
      <p className="text-lg mb-4">{novel.description}</p>
      <p className="text-sm text-gray-600 mb-4">المؤلف: {novel.author}</p>
      <p className="text-sm text-gray-600 mb-4">الحالة: {novel.status}</p>

      <h3 className="text-2xl font-bold mb-4">المجلدات المتاحة</h3>
      <div className="grid gap-4 md:grid-cols-2 lg:grid-cols-3">
        {[...Array(Math.ceil(novel.volumes))].map((_, i) => (
          <Card key={i} className="shadow border">
            <CardContent className="p-4 space-y-3">
              <h4 className="text-xl font-semibold">المجلد {i + 1}</h4>
              <Button className={`${novel.buttonColor} text-white w-full`} onClick={() => handleDownload(i + 1)}>
                <Download className="h-4 w-4 ml-2" /> تحميل المجلد
              </Button>
            </CardContent>
          </Card>
        ))}
      </div>
    </div>
  );
}

export default function App() {
  return (
    <Router>
      <Routes>
        <Route path="/roshsubs" element={<Home />} />
        <Route path="/roshsubs/novels/:novelTitle/volumes" element={<VolumeList />} />
      </Routes>
    </Router>
  );
}
