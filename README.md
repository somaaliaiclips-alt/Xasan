import { useState } from "react"; import { motion } from "framer-motion"; import jsPDF from "jspdf"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { Input } from "@/components/ui/input"; import { Textarea } from "@/components/ui/textarea"; import { Upload, Download, Printer, LogIn, Save } from "lucide-react";

const translations = { so: { title: "CV Builder Casri Ah", subtitle: "Buuxi xogtaada, beddel, kuna dar sawir.", save: "Kaydi CV", login: "Login", }, en: { title: "Modern CV Builder", subtitle: "Fill your info, edit and upload a photo.", save: "Save CV", login: "Login", }, };

const templates = { classic: "border-l-4 border-gray-800 pl-4", modern: "bg-gradient-to-br from-gray-50 to-white", minimal: "border border-dashed", };

export default function CVBuilderApp() { const [photo, setPhoto] = useState(null); const [lang, setLang] = useState("so"); const [template, setTemplate] = useState("classic"); const [loggedIn, setLoggedIn] = useState(false);

const [form, setForm] = useState({ name: "", title: "", email: "", phone: "", summary: "", experience: "", education: "", skills: "", });

const handleChange = (e) => { setForm({ ...form, [e.target.name]: e.target.value }); };

const handlePhotoUpload = (e) => { const file = e.target.files[0]; if (file) setPhoto(URL.createObjectURL(file)); };

const downloadPDF = () => { const pdf = new jsPDF(); pdf.text(CV - ${form.name}, 10, 10); pdf.text(JSON.stringify(form, null, 2), 10, 25); pdf.save("cv.pdf"); };

const saveToAccount = () => { if (!loggedIn) return alert("Please login first"); localStorage.setItem("savedCV", JSON.stringify({ form, photo })); alert("CV Saved Successfully"); };

const t = translations[lang];

return ( <div className="min-h-screen bg-gray-50 p-6 space-y-4"> {/* Header */} <div className="flex justify-between items-center"> <h1 className="text-3xl font-bold">{t.title}</h1> <div className="flex gap-2"> <Button variant="outline" onClick={() => setLang(lang === "so" ? "en" : "so")}>🌍 {lang.toUpperCase()}</Button> <Button variant="outline" onClick={() => setLoggedIn(!loggedIn)}><LogIn size={16} /> {t.login}</Button> </div> </div>

<div className="grid lg:grid-cols-2 gap-6">
    {/* Editor */}
    <Card className="rounded-2xl shadow-lg">
      <CardContent className="p-6 space-y-4">
        <p className="text-gray-500">{t.subtitle}</p>

        <div className="flex items-center gap-4">
          <label className="flex items-center gap-2 cursor-pointer">
            <Upload size={18} />
            <span>Upload Photo</span>
            <input type="file" hidden accept="image/*" onChange={handlePhotoUpload} />
          </label>
          {photo && <img src={photo} className="w-16 h-16 rounded-full object-cover" />}
        </div>

        <Input name="name" placeholder="Name" onChange={handleChange} />
        <Input name="title" placeholder="Title" onChange={handleChange} />
        <Input name="email" placeholder="Email" onChange={handleChange} />
        <Input name="phone" placeholder="Phone" onChange={handleChange} />

        <Textarea name="summary" placeholder="Summary" onChange={handleChange} />
        <Textarea name="experience" placeholder="Experience" onChange={handleChange} />
        <Textarea name="education" placeholder="Education" onChange={handleChange} />
        <Textarea name="skills" placeholder="Skills" onChange={handleChange} />

        {/* Template Selector */}
        <div className="flex gap-2">
          {Object.keys(templates).map((t) => (
            <Button key={t} variant={template === t ? "default" : "outline"} onClick={() => setTemplate(t)}>
              {t}
            </Button>
          ))}
        </div>

        <div className="flex gap-2">
          <Button className="w-full" onClick={saveToAccount}><Save size={16} /> {t.save}</Button>
          <Button variant="outline" onClick={downloadPDF}><Download size={16} /> PDF</Button>
          <Button variant="outline" onClick={() => window.print()}><Printer size={16} /> Print</Button>
        </div>
      </CardContent>
    </Card>

    {/* Preview */}
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      className={`bg-white rounded-2xl shadow-lg p-6 ${templates[template]}`}
    >
      <div className="flex gap-4 items-center">
        {photo && <img src={photo} className="w-24 h-24 rounded-full object-cover" />}
        <div>
          <h2 className="text-2xl font-bold">{form.name || "Your Name"}</h2>
          <p className="text-gray-500">{form.title || "Profession"}</p>
          <p className="text-sm">{form.email}</p>
          <p className="text-sm">{form.phone}</p>
        </div>
      </div>

      <section className="mt-6"><h3 className="font-semibold">Summary</h3><p>{form.summary}</p></section>
      <section className="mt-4"><h3 className="font-semibold">Experience</h3><p>{form.experience}</p></section>
      <section className="mt-4"><h3 className="font-semibold">Education</h3><p>{form.education}</p></section>
      <section className="mt-4"><h3 className="font-semibold">Skills</h3><p>{form.skills}</p></section>
    </motion.div>
  </div>
</div>

); }
