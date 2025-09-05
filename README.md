<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI Resume + Portfolio</title>
  <!-- Tailwind CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- React & ReactDOM CDN -->
  <script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
  <!-- Babel for JSX -->
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <!-- Recharts CDN -->
  <script src="https://unpkg.com/recharts/umd/Recharts.min.js"></script>
  <!-- Framer Motion CDN -->
  <script src="https://unpkg.com/framer-motion/dist/framer-motion.umd.js"></script>
</head>
<body class="bg-gray-100 dark:bg-gray-900 transition-colors duration-300 font-sans">

<div id="root"></div>

<script type="text/babel">

const { useState } = React;
const { motion } = window['framer-motion'];
const { PieChart, Pie, Tooltip, Legend, Cell, ResponsiveContainer } = Recharts;

function App() {
  const [darkMode, setDarkMode] = useState(false);
  const [resumeFile, setResumeFile] = useState(null);
  const [resumeContent, setResumeContent] = useState('');
  const [selectedRole, setSelectedRole] = useState("");
  const [selectedKeyword, setSelectedKeyword] = useState("");
  const [quizResults, setQuizResults] = useState({});

  const toggleTheme = () => setDarkMode(!darkMode);

  const handleFileUpload = (event) => {
    const file = event.target.files[0];
    if (file) {
      setResumeFile(file);
      const reader = new FileReader();
      reader.onload = (e) => {
        setResumeContent(e.target.result);
      };
      reader.readAsText(file); // Reads text content
    }
  };

  const jobRoles = {
    "Data Analyst": ["SQL", "Excel", "Data Visualization"],
    "AI Engineer": ["Machine Learning", "Python", "MLOps"],
    "Software Developer": ["React", "System Design", "Testing"],
    "Product Manager": ["Roadmapping", "Communication", "Stakeholder Management"],
    "UX Designer": ["Wireframing", "Prototyping", "User Research"]
  };

  const keywords = [
    { word: "SQL", insight: "Database querying, joins, and optimization skills." },
    { word: "React", insight: "Frontend development, component architecture." },
    { word: "Python", insight: "Scripting, automation, ML model building." },
    { word: "Machine Learning", insight: "Supervised, unsupervised learning techniques." },
    { word: "Data Visualization", insight: "Charts, dashboards, and storytelling with data." }
  ];

  const careerRoadmaps = {
    "Data Analyst": ["Internship", "Junior Analyst", "Senior Analyst", "Lead Analyst"],
    "AI Engineer": ["AI Intern", "Junior ML Engineer", "ML Engineer", "Senior AI Engineer"],
    "Software Developer": ["Intern Developer", "Junior Developer", "Developer", "Senior Developer"],
    "Product Manager": ["Associate PM", "PM", "Senior PM", "Head of Product"],
    "UX Designer": ["Junior Designer", "Designer", "Senior Designer", "Lead UX Designer"]
  };

  const pieData = {
    "Data Analyst": [
      { name: 'SQL', value: 30 },
      { name: 'Excel', value: 25 },
      { name: 'Data Visualization', value: 45 }
    ],
    "AI Engineer": [
      { name: 'Python', value: 40 },
      { name: 'Machine Learning', value: 35 },
      { name: 'MLOps', value: 25 }
    ],
    "Software Developer": [
      { name: 'React', value: 35 },
      { name: 'System Design', value: 40 },
      { name: 'Testing', value: 25 }
    ],
    "Product Manager": [
      { name: 'Roadmapping', value: 40 },
      { name: 'Communication', value: 35 },
      { name: 'Stakeholder Mgmt', value: 25 }
    ],
    "UX Designer": [
      { name: 'Wireframing', value: 30 },
      { name: 'Prototyping', value: 35 },
      { name: 'User Research', value: 35 }
    ]
  };

  const quizQuestions = [
    {
      question: "Which SQL command is used to retrieve data?",
      options: ["SELECT", "INSERT", "UPDATE"],
      answer: "SELECT"
    },
    {
      question: "Which lifecycle method is used to render UI in React?",
      options: ["componentDidMount", "render", "useEffect"],
      answer: "render"
    },
    {
      question: "Which Python library is used for data analysis?",
      options: ["NumPy", "React", "Express"],
      answer: "NumPy"
    }
  ];

  const handleQuizAnswer = (question, option) => {
    setQuizResults(prev => ({ ...prev, [question]: option }));
  };

  const COLORS = ['#FF0000', '#00FF00', '#000000'];

  return (
    <div className={`${darkMode ? "bg-gray-900 text-gray-100" : "bg-gray-100 text-gray-900"} transition-colors duration-300`}>
      <nav className="flex justify-between items-center p-6 shadow-md bg-gradient-to-r from-blue-500 to-purple-500 text-white">
        <h1 className="text-2xl font-bold">AI Resume + Portfolio</h1>
        <button onClick={toggleTheme} className="px-4 py-2 bg-white text-black rounded-full shadow-md hover:scale-105 transition">
          {darkMode ? "Switch to Light Mode" : "Switch to Dark Mode"}
        </button>
      </nav>

      <section className="flex flex-col items-center justify-center py-16 px-4 text-center bg-gradient-to-b from-pink-400 to-yellow-300 dark:from-gray-800 dark:to-gray-700">
        <h2 className="text-4xl font-extrabold mb-4">Welcome to Your Interactive Career Hub</h2>
        <p className="max-w-2xl text-lg mb-6">Upload your resume from your computer to explore interactive insights.</p>
        <div className="flex gap-4">
          <label className="px-6 py-3 bg-blue-600 text-white rounded-xl shadow-md cursor-pointer hover:scale-105 transition">
            Upload Resume
            <input type="file" accept=".txt,.pdf,.doc,.docx" className="hidden" onChange={handleFileUpload} />
          </label>
        </div>
        {resumeFile && <p className="mt-4">Uploaded: {resumeFile.name}</p>}
        {resumeContent && <pre className="mt-4 max-h-60 overflow-auto bg-gray-200 dark:bg-gray-800 p-2 rounded text-left">{resumeContent}</pre>}
      </section>

      <section className="py-16 px-6 text-center">
        <h3 className="text-3xl font-semibold mb-6">Resume Keyword Explorer</h3>
        <div className="flex flex-wrap justify-center gap-4">
          {keywords.map((kw, idx) => (
            <button key={idx} onClick={() => setSelectedKeyword(kw.word)} className="px-4 py-2 bg-gradient-to-br from-purple-300 to-pink-300 dark:from-gray-800 dark:to-gray-700 rounded-xl shadow-md hover:scale-105 transition">
              {kw.word}
            </button>
          ))}
        </div>
        {selectedKeyword && <p className="mt-4">Insight: <span className="font-bold">{keywords.find(k => k.word === selectedKeyword).insight}</span></p>}
      </section>

      <section className="py-16 px-6 text-center bg-gradient-to-r from-green-400 to-blue-400 dark:from-gray-700 dark:to-gray-600">
        <h3 className="text-3xl font-semibold mb-6">Career Role Simulator</h3>
        <div className="flex justify-center gap-4 flex-wrap">
          {Object.keys(jobRoles).map((role, idx) => (
            <button key={idx} onClick={() => setSelectedRole(role)} className="px-6 py-3 bg-blue-600 text-white rounded-xl shadow-md hover:scale-105 transition">
              {role}
            </button>
          ))}
        </div>
        {selectedRole && (
          <div className="mt-6 max-w-lg mx-auto bg-white dark:bg-gray-900 p-6 rounded-xl shadow-md text-left text-gray-900 dark:text-gray-100">
            <h4 className="font-semibold mb-2">Required Skills for {selectedRole}:</h4>
            <ul className="list-disc list-inside">
              {jobRoles[selectedRole].map((skill, i) => <li key={i}>{skill}</li>)}
            </ul>
          </div>
        )}
      </section>

      {selectedRole && (
        <section className="py-16 px-6 text-center">
          <h3 className="text-3xl font-semibold mb-6">Career Path Roadmap</h3>
          <div className="flex flex-col md:flex-row justify-center items-center gap-8 overflow-x-auto">
            <div style={{ width: 400, height: 300 }}>
              <PieChart width={400} height={300}>
                <Pie data={pieData[selectedRole]} dataKey="value" nameKey="name" cx="50%" cy="50%" outerRadius={100} label>
                  {pieData[selectedRole].map((entry, index) => (
                    <Cell key={`cell-${index}`} fill={COLORS[index % COLORS.length]} />
                  ))}
                </Pie>
                <Tooltip />
                <Legend />
              </PieChart>
            </div>
            <div className="flex flex-col gap-4 md:w-1/2">
              {careerRoadmaps[selectedRole].map((stage, idx) => (
                <motion.div key={idx} initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }} transition={{ delay: idx * 0.3 }} className="flex flex-col items-center bg-gradient-to-br from-indigo-400 to-purple-400 dark:from-gray-800 dark:to-gray-700 text-white rounded-xl p-6 shadow-lg">
                  <span className="font-bold text-lg mb-2">{stage}</span>
                  <span className="text-sm">Tips & milestones here</span>
                </motion.div>
              ))}
            </div>
          </div>
        </section>
      )}

      {selectedRole && (
        <section className="py-16 px-6 text-center bg-gradient-to-r from-yellow-400 to-pink-400 dark:from-gray-800 dark:to-gray-700">
          <h3 className="text-3xl font-semibold mb-6">Interview Prep Simulator</h3>
          <div className="grid md:grid-cols-3 gap-6 max-w-5xl mx-auto">
            {quizQuestions.map((q, idx) => (
              <div key={idx} className="p-6 bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 rounded-xl shadow-md">
                <h4 className="font-bold mb-2">{q.question}</h4>
                {q.options.map((opt, i) => (
                  <button key={i} onClick={() => handleQuizAnswer(q.question, opt)} className="m-1 px-3 py-1 bg-blue-500 text-white rounded hover:scale-105 transition">
                    {opt}
                  </button>
                ))}
                {quizResults[q.question] && (
                  <p className="mt-2">
                    Selected Answer: <span className="font-bold">{quizResults[q.question]}</span> — {quizResults[q.question] === q.answer ? <span className="text-green-600">Correct</span> : <span className="text-red-600">Incorrect</span>}
                  </p>
                )}
              </div>
            ))}
          </div>
        </section>
      )}
    </div>
  );
}

ReactDOM.render(<App />, document.getElementById('root'));

</script>

</body>
</html>
