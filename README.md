import React, { useState, useEffect, useRef } from 'react';
import { 
  BookOpen, 
  Sparkles, 
  FileText, 
  Layers, 
  CheckSquare, 
  Shuffle, 
  Image as ImageIcon, 
  Home, 
  Download, 
  Clipboard, 
  Bookmark, 
  Clock, 
  HelpCircle,
  Eye,
  Check,
  AlertCircle,
  Play,
  Pause,
  Moon,
  Sun,
  Trash2,
  ChevronRight,
  Info,
  Users,
  Timer,
  Plus,
  RotateCcw,
  Link2,
  Youtube,
  Globe,
  ExternalLink,
  Edit2,
  Search,
  Maximize2,
  Minimize2,
  Volume2,
  Award,
  PenTool,
  CornerDownRight,
  BookOpenCheck,
  BookmarkCheck,
  ChevronLeft,
  Settings,
  HeartHandshake
} from 'lucide-react';

// Сыныптар мен оларға тиесілі пәндер тізімі (ҚР стандарты)
const SUBJECTS_BY_GRADE = {
  "1-сынып": ["Әліппе / Ана тілі", "Математика", "Жаратылыстану", "Цифрлық сауаттылық және жасанды интеллект", "Музыка", "Бейнелеу өнері", "Еңбекке баулу", "Дене шынықтыру"],
  "2-сынып": ["Қазақ тілі", "Әдебиеттік оқу", "Математика", "Жаратылыстану", "Дүниетану", "Цифрлық сауаттылық және жасанды интеллект", "Музыка", "Бейнелеу өнері", "Еңбекке баулу", "Дене шынықтыру"],
  "3-сынып": ["Қазақ тілі", "Әдебиеттік оқу", "Орыс тілі", "Ағылшын тілі", "Математика", "Жаратылыстану", "Дүниетану", "Цифрлық сауаттылық және жасанды интеллект", "Музыка", "Бейнелеу өнері", "Еңбекке баулу", "Дене шынықтыру"],
  "4-сынып": ["Қазақ тілі", "Әдебиеттік оқу", "Орыс тілі", "Ағылшын тілі", "Математика", "Жаратылыстану", "Дүниетану", "Цифрлық сауаттылық және жасанды интеллект", "Музыка", "Бейнелеу өнері", "Еңбекке баулу", "Дене шынықтыру"],
  "5-сынып": ["Қазақ тілі", "Қазақ әдебиеті", "Орыс тілі", "Ағылшын тілі", "Математика", "Жаратылыстану", "Қазақстан тарихы", "Дүниежүзі тарихы", "Информатика және жасанды интеллект", "Музыка", "Көркем еңбек", "Дене шынықтыру"],
  "6-сынып": ["Қазақ тілі", "Қазақ әдебиеті", "Орыс тілі", "Ағылшын тілі", "Математика", "Жаратылыстану", "Қазақстан тарихы", "Дүниежүзі тарихы", "Информатика және жасанды интеллект", "Музыка", "Көркем еңбек", "Дене шынықтыру"],
  "7-сынып": ["Қазақ тілі", "Қазақ әдебиеті", "Орыс тілі", "Ағылшын тілі", "Алгебра", "Геометрия", "Физика", "Химия", "Биология", "География", "Қазақстан тарихы", "Дүниежүзі тарихы", "Информатика және жасанды интеллект", "Көркем еңбек", "Дене шынықтыру"],
  "8-сынып": ["Қазақ тілі", "Қазақ әдебиеті", "Орыс тілі", "Ағылшын тілі", "Алгебра", "Геометрия", "Физика", "Химия", "Биология", "География", "Қазақстан тарихы", "Дүниежүзі тарихы", "Информатика және жасанды интеллект", "Көркем еңбек", "Дене шынықтыру"],
  "9-сынып": ["Қазақ тілі", "Қазақ әдебиеті", "Орыс тілі", "Ағылшын тілі", "Алгебра", "Геометрия", "Физика", "Химия", "Биология", "География", "Қазақстан тарихы", "Дүниежүзі тарихы", "Құқық негіздері", "Зайырлылық және дінтану негіздері", "Информатика және жасанды интеллект", "Көркем еңбек", "Дене шынықтыру"],
  "10-сынып": ["Қазақ тілі", "Қазақ әдебиеті", "Орыс тілі", "Ағылшын тілі", "Алгебра және анализ бастамалары", "Геометрия", "Физика", "Химия", "Биология", "География", "Қазақстан тарихы", "Дүниежүзі тарихы", "Құқық негіздері", "Кәсіпкерлік және бизнес негіздері", "Информатика және жасанды интеллект", "АӘТД", "Дене шынықтыру"],
  "11-сынып": ["Қазақ тілі", "Қазақ әдебиеті", "Орыс тілі", "Ағылшын тілі", "Алгебра және анализ бастамалары", "Геометрия", "Физика", "Химия", "Биология", "География", "Қазақстан тарихы", "Дүниежүзі тарихы", "Құқық негіздері", "Кәсіпкерлік және бизнес негіздері", "Информатика және жасанды интеллект", "АӘТД", "Дене шынықтыру"]
};

const GRADES = Object.keys(SUBJECTS_BY_GRADE);

// Жылдам сынап көруге арналған дайын тақырыптар шаблоны
const TEMPLATES = [
  { topic: "Абай Құнанбаевтың өмірі мен шығармашылығы", subject: "Қазақ әдебиеті", grade: "9-сынып" },
  { topic: "Фотосинтез және оның маңызы", subject: "Биология", grade: "8-сынып" },
  { topic: "Квадрат теңдеулерді шешу жолдары", subject: "Алгебра", grade: "8-сынып" },
  { topic: "Қазақ хандығының құрылуы", subject: "Қазақстан тарихы", grade: "7-сынып" },
  { topic: "Заттың агрегаттық күйлері", subject: "Физика", grade: "7-сынып" }
];

export default function App() {
  // Форма күйлері
  const [topic, setTopic] = useState('');
  const [grade, setGrade] = useState('5-сынып');
  const [subject, setSubject] = useState(SUBJECTS_BY_GRADE['5-сынып'][0]);
  const [additionalNotes, setAdditionalNotes] = useState('');

  // AI-мен жұмыс істеу күйлері
  const [isGenerating, setIsGenerating] = useState(false);
  const [generationStep, setGenerationStep] = useState('');
  const [apiError, setApiError] = useState(null);
  const [generatedData, setGeneratedData] = useState(null);
  const [activeTab, setActiveTab] = useState('plan'); // plan, slides, test, matching, assessment, visuals, homework, tools, resources
  const [currentSlideIndex, setCurrentSlideIndex] = useState(0);
  const [isSlideFullscreen, setIsSlideFullscreen] = useState(false);

  // Сурет генерациялау күйлері (Imagen)
  const [isGeneratingImage, setIsGeneratingImage] = useState(false);
  const [generatedImageUrl, setGeneratedImageUrl] = useState('');
  const [imageError, setImageError] = useState(null);

  // Тарих пен Сақталғандар
  const [savedLessons, setSavedLessons] = useState([]);
  const [notification, setNotification] = useState(null);
  
  // Тест тапсыру және интерактивті ойын күйлері
  const [testAnswers, setTestAnswers] = useState({});
  const [testChecked, setTestChecked] = useState(false);
  const [matchingSelections, setMatchingSelections] = useState({ left: null, right: null });
  const [matchedPairs, setMatchedPairs] = useState([]);
  const [matchingFeedback, setMatchingFeedback] = useState(null);
  const [shuffledLeft, setShuffledLeft] = useState([]);
  const [shuffledRight, setShuffledRight] = useState([]);

  // Сыныптық басқару құралдары күйлері (Classroom Tools)
  const [students, setStudents] = useState([]);
  const [newStudent, setNewStudent] = useState("");
  const [selectedStudent, setSelectedStudent] = useState('');
  const [isSpinning, setIsSpinning] = useState(false);
  const [generatedGroups, setGeneratedGroups] = useState(null);
  const [numberOfGroups, setNumberOfGroups] = useState(2);
  const [timerSeconds, setTimerSeconds] = useState(300);
  const [isTimerRunning, setIsTimerRunning] = useState(false);
  const [customMinutes, setCustomMinutes] = useState('5');

  // Мұғалімнің жеке блокноты (Scratchpad)
  const [scratchpadText, setScratchpadText] = useState('');
  const [isScratchpadOpen, setIsScratchpadOpen] = useState(false);

  // Пайдаланушы енгізетін іздеу сөздерін түзету мен қосу күйлері
  const [customYoutubeQuery, setCustomYoutubeQuery] = useState('');
  const [customGamesQuery, setCustomGamesQuery] = useState('');
  const [customWebQuery, setCustomWebQuery] = useState('');

  // Мұғалімнің сілтемелерді қолмен өңдеу және қосу күйлері
  const [editingResource, setEditingResource] = useState(null); // { category: 'youtube'|'games'|'websites', index: number }
  const [editForm, setEditForm] = useState({ title: '', url: '', desc: '', platformOrSource: '' });
  const [addingToCategory, setAddingToCategory] = useState(null); // 'youtube' | 'games' | 'websites'

  // Қараңғы режим күйі
  const [darkMode, setDarkMode] = useState(false);

  // Сабақтарды және блокнот мәтінін жүктеу
  useEffect(() => {
    const localData = localStorage.getItem('ustaz_saved_lessons');
    if (localData) {
      try {
        setSavedLessons(JSON.parse(localData));
      } catch (e) {
        console.error("Сақталған сабақтарды жүктеу қатесі", e);
      }
    }
    const savedScratch = localStorage.getItem('ustaz_scratchpad');
    if (savedScratch) {
      setScratchpadText(savedScratch);
    }
    const savedStudents = localStorage.getItem('ustaz_students');
    if (savedStudents) {
      try {
        setStudents(JSON.parse(savedStudents));
      } catch (e) {
        console.error("Оқушылар тізімін жүктеу қатесі", e);
      }
    }
  }, []);

  // Блокнот мәтіні өзгерген сайын сақтау
  const handleScratchpadChange = (val) => {
    setScratchpadText(val);
    localStorage.setItem('ustaz_scratchpad', val);
  };

  // Сабақ уақытының кері санақ таймері
  useEffect(() => {
    let interval = null;
    if (isTimerRunning && timerSeconds > 0) {
      interval = setInterval(() => {
        setTimerSeconds(prev => prev - 1);
      }, 1000);
    } else if (timerSeconds === 0 && isTimerRunning) {
      setIsTimerRunning(false);
      showNotification('Уақыт аяқталды!', 'info');
    }
    return () => clearInterval(interval);
  }, [isTimerRunning, timerSeconds]);

  const handleCustomTimer = () => {
    const mins = parseInt(customMinutes);
    if (!isNaN(mins) && mins > 0) {
      setTimerSeconds(mins * 60);
      setIsTimerRunning(false);
      showNotification(`Таймер ${mins} минутқа орнатылды`, 'info');
    } else {
      showNotification('Жарамды минут саням енгізіңіз', 'error');
    }
  };

  const showNotification = (message, type = 'success') => {
    setNotification({ message, type });
    setTimeout(() => setNotification(null), 4000);
  };

  const copyToClipboard = (text, message = 'Мәтін алмасу буферіне көшірілді!') => {
    const textArea = document.createElement("textarea");
    textArea.value = text;
    document.body.appendChild(textArea);
    textArea.select();
    try {
      document.execCommand('copy');
      showNotification(message, 'success');
    } catch (err) {
      showNotification('Көшіру сәтсіз аяқталды', 'error');
    }
    document.body.removeChild(textArea);
  };

  // Мәтінді дауыстап оқу (Web Speech API) Инклюзивті қолдау
  const speakText = (text) => {
    if ('speechSynthesis' in window) {
      window.speechSynthesis.cancel(); // Алдыңғы оқылымды тоқтату
      const utterance = new SpeechSynthesisUtterance(text);
      const voices = window.speechSynthesis.getVoices();
      
      // Қазақ немесе орыс дауысын табуға талпыну
      const targetVoice = voices.find(v => v.lang.includes('kk') || v.lang.includes('ru') || v.lang.includes('KK'));
      if (targetVoice) {
        utterance.voice = targetVoice;
      }
      utterance.lang = 'kk-KZ';
      utterance.rate = 0.95; // Оқу жылдамдығы сәл баяулау
      window.speechSynthesis.speak(utterance);
      showNotification('Дауыстық көмекші қосылды', 'info');
    } else {
      showNotification('Сіздің браузеріңіз дауыстық сипаттаманы қолдамайды', 'error');
    }
  };

  const saveLessonToHistory = (lesson) => {
    const updated = [lesson, ...savedLessons.filter(l => l.id !== lesson.id)].slice(0, 15);
    setSavedLessons(updated);
    localStorage.setItem('ustaz_saved_lessons', JSON.stringify(updated));
    showNotification('Сабақ сәтті сақталды!', 'success');
  };

  const deleteLessonFromHistory = (id, e) => {
    e.stopPropagation();
    const updated = savedLessons.filter(l => l.id !== id);
    setSavedLessons(updated);
    localStorage.setItem('ustaz_saved_lessons', JSON.stringify(updated));
    showNotification('Сабақ тізімнен өшірілді.', 'info');
  };

  const applyTemplate = (tpl) => {
    setTopic(tpl.topic);
    setGrade(tpl.grade);
    
    // Шаблон пәнін таңдалған сыныптың тізімінен тексеру
    const availableSubjects = SUBJECTS_BY_GRADE[tpl.grade] || [];
    if (availableSubjects.includes(tpl.subject)) {
      setSubject(tpl.subject);
    } else {
      setSubject(availableSubjects[0] || tpl.subject);
    }
    
    showNotification('Үлгі мәліметтері енгізілді!', 'info');
  };

  // Мұғалімнің ресурсты өзгерту логикасы
  const handleStartEditResource = (category, index, item) => {
    setEditingResource({ category, index });
    setEditForm({
      title: item.title || '',
      url: item.searchUrl || item.url || '',
      desc: item.description || '',
      platformOrSource: item.platform || item.sourceName || ''
    });
  };

  const handleSaveResourceEdit = () => {
    if (!generatedData) return;
    const updatedData = { ...generatedData };
    const categoryList = [...updatedData.externalResources[editingResource.category]];
    
    const item = categoryList[editingResource.index];
    const updatedItem = {
      ...item,
      title: editForm.title,
    };
    
    if (editingResource.category === 'youtube' || editingResource.category === 'games') {
      updatedItem.searchUrl = editForm.url;
    } else {
      updatedItem.url = editForm.url;
    }

    if (editingResource.category === 'games') {
      updatedItem.platform = editForm.platformOrSource;
      updatedItem.description = editForm.desc;
    } else if (editingResource.category === 'websites') {
      updatedItem.sourceName = editForm.platformOrSource;
    }

    categoryList[editingResource.index] = updatedItem;
    updatedData.externalResources[editingResource.category] = categoryList;
    
    setGeneratedData(updatedData);
    setEditingResource(null);
    showNotification('Ресурс сәтті жаңартылды!', 'success');
  };

  const handleAddCustomResource = (category) => {
    if (!generatedData) return;
    
    const newResource = {
      title: editForm.title || 'Жаңа ресурс',
    };

    if (category === 'youtube' || category === 'games') {
      newResource.searchUrl = editForm.url || 'https://';
      newResource.queryText = editForm.title;
    } else {
      newResource.url = editForm.url || 'https://';
      newResource.queryText = editForm.title;
    }

    if (category === 'games') {
      newResource.platform = editForm.platformOrSource || 'Wordwall';
      newResource.description = editForm.desc || 'Пайдаланушы қосқан интерактивті ойын';
    } else if (category === 'websites') {
      newResource.sourceName = editForm.platformOrSource || 'Веб-сайт';
    }

    const updatedData = { ...generatedData };
    if (!updatedData.externalResources) {
      updatedData.externalResources = { youtube: [], games: [], websites: [] };
    }
    if (!updatedData.externalResources[category]) {
      updatedData.externalResources[category] = [];
    }

    updatedData.externalResources[category] = [...updatedData.externalResources[category], newResource];
    setGeneratedData(updatedData);
    setAddingToCategory(null);
    setEditForm({ title: '', url: '', desc: '', platformOrSource: '' });
    showNotification('Жаңа ресурс сәтті қосылды!', 'success');
  };

  const handleDeleteResource = (category, index) => {
    if (!generatedData) return;
    const updatedData = { ...generatedData };
    updatedData.externalResources[category] = updatedData.externalResources[category].filter((_, idx) => idx !== index);
    setGeneratedData(updatedData);
    showNotification('Ресурс тізімнен өшірілді', 'info');
  };

  const prepareMatchingGame = (matchingArray) => {
    if (!matchingArray || !Array.isArray(matchingArray)) return;
    // Сол жақ пен Оң жақты бөлек алып, араластыру
    const leftSide = matchingArray.map((item, idx) => ({ id: idx, text: item.left }));
    const rightSide = matchingArray.map((item, idx) => ({ id: idx, text: item.right }));
    
    // Араластыру функциясы (Fisher-Yates)
    const shuffle = (array) => {
      const arr = [...array];
      for (let i = arr.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [arr[i], arr[j]] = [arr[j], arr[i]];
      }
      return arr;
    };

    setShuffledLeft(shuffle(leftSide));
    setShuffledRight(shuffle(rightSide));
    setMatchedPairs([]);
    setMatchingSelections({ left: null, right: null });
  };

  const generateLessonMaterial = async () => {
    if (!topic.trim()) {
      setApiError('Сабақтың тақырыбын енгізіңіз!');
      return;
    }

    setIsGenerating(true);
    setApiError(null);
    setGeneratedData(null);
    setGeneratedImageUrl('');
    setTestAnswers({});
    setTestChecked(false);
    setMatchedPairs([]);
    setCurrentSlideIndex(0);
    
    setCustomYoutubeQuery('');
    setCustomGamesQuery('');
    setCustomWebQuery('');
    
    const steps = [
      "Сабақ жоспарын құрылымдау (ҚМЖ)...",
      "Презентациялық слайдтар мен көрнекілікті даярлау...",
      "Тақырыптық бағалау дескрипторларын генерациялау...",
      "Интерактивті тест сұрақтарын құрастыру...",
      "Сәйкестендіру терминдік ойынын әзірлеу...",
      "Үй тапсырмасы мен инклюзивті оқыту стратегияларын өңдеу...",
      "YouTube және отандық білім сайттарының сілтемелерін бекіту..."
    ];
    
    let stepIndex = 0;
    setGenerationStep(steps[0]);
    const stepInterval = setInterval(() => {
      stepIndex = (stepIndex + 1) % steps.length;
      setGenerationStep(steps[stepIndex]);
    }, 2000);

    const systemPrompt = `Сіз тәжірибелі қазақстандық әдіскер-мұғалімсіз. Берілген сабақ тақырыбы бойынша барлық оқу материалдарын қазақ тілінде, Қазақстанның орта білім беру стандартына (ҚМЖ/ҰМЖ сәйкес) сай дайындаңыз.
Жауапты СТРУКТУРАЛАНҒАН және тек қана валидті JSON форматында қайтарыңыз. Мәтінде ешқандай басқа артық сөз болмауы тиіс. JSON пішімі келесідей құрылымда болуы керек:

{
  "lessonPlan": {
    "title": "Сабақтың толық тақырыбы",
    "subject": "Пән атауы",
    "grade": "Сынып",
    "learningObjectives": ["Осы сабақта қол жеткізілетін оқу мақсаттары (оқу бағдарламасына сілтеме)"],
    "lessonObjectives": {
      "all": "Барлық оқушылар жасай алады:",
      "most": "Оқушылардың басым бөлігі жасай алады:",
      "some": "Кейбір оқушылар жасай алады:"
    },
    "assessmentCriteria": ["Бағалау критерийі 1", "Бағалау критерийі 2"],
    "values": "Құндылықтарды дарыту (мысалы: Ұлттық мүдде, Ар-ұят, Талап)",
    "crossCurricular": "Пәнаралық байланыс",
    "stages": [
      {
        "name": "Сабақтың басы / Ұйымдастыру",
        "duration": "10 мин",
        "teacherActivity": "Педагогтің іс-әрекеті",
        "studentActivity": "Оқушының іс-әрекеті",
        "assessment": "Бағалау (ҚБ)",
        "resources": "Ресурстар (слайд, оқулық т.б.)"
      },
      {
        "name": "Сабақтың ортасы / Жаңа сабақ",
        "duration": "25 мин",
        "teacherActivity": "Мұғалімнің іс-әрекеті (тапсырмалар беру, түсіндіру)",
        "studentActivity": "Оқушының іс-әрекеті",
        "assessment": "Бағалау (Дескриптор бойынша)",
        "resources": "Жұмыс дәптері, үлестірмелі материал"
      },
      {
        "name": "Сабақтың соңы / Рефлексия",
        "duration": "10 мин",
        "teacherActivity": "Рефлексия ұйымдастыру, үй тапсырмасы",
        "studentActivity": "Кері байланыс беру",
        "assessment": "Жиынтық немесе өзара бағалау",
        "resources": "Рефлексия парағы"
      }
    ]
  },
  "assessmentRubrics": [
    {
      "criteria": "Бағалау критерийі (мысалы, Тақырып бойынша терминдерді қолдана алу)",
      "descriptors": [
        "Төмен: Мәліметтерді шатастырады немесе сипаттай алмайды",
        "Орта: Кейбір анықтамаларды дұрыс қолданады, бірақ қателіктер жібереді",
        "Жоғары: Барлық терминдерді қатесіз біледі, жүйелі талдайды"
      ]
    },
    {
      "criteria": "Тәжірибелік немесе талдау дағдысы",
      "descriptors": [
        "Төмен: Сұрақтарға жауап беруде қиналады",
        "Орта: Тақырыпты сипаттай алады, бірақ толық түсіндіре алмайды",
        "Жоғары: Сұрақтарға еркін жауап беріп, өмірмен байланыстыра алады"
      ]
    }
  ],
  "exitTicket": {
    "title": "3-2-1 Рефлексия әдісі",
    "bullets": [
      "3 маңызды мәлімет (бүгін үйренген)",
      "2 қызықты сұрақ немесе идея",
      "1 қиындық тудырған сәт (немесе басты түсінік)"
    ]
  },
  "senAdaptation": "Ерекше білім беруді қажет ететін (ЕБҚ) оқушыларға арналған дифференциацияланған бейімдеу кеңесі (оқу материалын жеңілдету немесе көмекші суреттер ұсыну жолдары).",
  "slides": [
    {
      "slideNum": 1,
      "title": "Слайд тақырыбы",
      "bullets": ["Маңызды факт немесе сипаттама 1", "Маңызды факт немесе сипаттама 2"],
      "teacherNotes": "Осы слайдты түсіндіргенде мұғалімге айтуға кеңес"
    }
  ],
  "test": [
    {
      "question": "Сұрақ мәтіні?",
      "options": ["А нұсқасы", "В нұсқасы", "С нұсқасы", "D нұсқасы"],
      "answer": "Дұрыс нұсқа жауабы"
    }
  ],
  "matching": [
    {
      "left": "Сол жақтағы ұғым/термин",
      "right": "Оң жақтағы сипаттама/анықтама"
    }
  ],
  "visualPrompt": "A highly detailed educational poster or illustration about [describe the topic in simple terms] for classroom teaching, clean scientific diagram style, vibrant colors, clear shapes, high quality, 4k resolution.",
  "visualDescriptionKazakh": "Көрнекілік суреттің қазақша сипаттамасы және оны сабақта қалай қолдану керектігі туралы әдістемелік кеңес.",
  "practicalTasks": {
    "individual": "Жеке жұмысқа арналған тапсырма (дескрипторларымен бірге).",
    "pair": "Жұптық жұмысқа арналған тапсырма (өзара талқылау немесе тексеруге негізделген).",
    "group": "Топтық жұмысқа арналған тапсырма (рөлдерге бөлу немесе жобалық жұмыс)."
  },
  "homework": "Оқушыларға арналған деңгейлік үй тапсырмасы және оны орындау нұсқаулығы.",
  "externalResources": {
    "youtube": [
      {
        "title": "Тақырып бойынша ең жақсы қазақша түсіндірме бейнебаян іздеу жолы",
        "queryText": "Фотосинтез биология қазақша сабақ",
        "searchUrl": "https://www.youtube.com/results?search_query=Фотосинтез+биология+қазақша+сабақ"
      },
      {
        "title": "Тақырыпты көрнекі көрсететін 3D анимация немесе орысша/ағылшынша сапалы ресурс",
        "queryText": "Photosynthesis 3d animation school",
        "searchUrl": "https://www.youtube.com/results?search_query=Photosynthesis+3d+animation+school"
      }
    ],
    "games": [
      {
        "title": "Wordwall-дағы тақырыптық терминологиялық интерактивті ойын",
        "platform": "Wordwall",
        "description": "Сөздерді сәйкестендіру немесе ұшақ ойыны арқылы терминдерді жылдам қайталау құралы.",
        "queryText": "Фотосинтез биология",
        "searchUrl": "https://wordwall.net/ru/community?query=Фотосинтез+биология"
      },
      {
        "title": "Kahoot немесе LearningApps бойынша викторина іздеу",
        "platform": "LearningApps/Kahoot",
        "description": "Сынып арасында ұжымдық немесе жеке бағалау сайысын өткізуге арналған дайын сұрақтар.",
        "queryText": "Фотосинтез тест",
        "searchUrl": "https://learningapps.org/index.php?s=Фотосинтез"
      }
    ],
    "websites": [
      {
        "title": "BilimLand білім беру порталындағы теориялық материал",
        "sourceName": "BilimLand.kz",
        "queryText": "Фотосинтез bilimland",
        "url": "https://www.google.com/search?q=Фотосинтез+bilimland"
      },
      {
        "title": "Daryn.online немесе Википедиядағы тереңірек ғылыми сипаттама",
        "sourceName": "Wikipedia / Daryn",
        "queryText": "Фотосинтез қазақша мақала",
        "url": "https://www.google.com/search?q=Фотосинтез+қазақша+уикипедия"
      }
    ]
  }
}

МАҢЫЗДЫ САНДЫҚ ТАЛАПТАР:
1. "slides" массивінде КЕМІНДЕ 5-6 СЛАЙД болуы тиіс.
2. "test" массивінде КЕМІНДЕ 5 ТЕСТ СҰРАҒЫ болуы тиіс.
3. "matching" массивінде КЕМІНДЕ 5 СӘЙКЕСТЕНДІРУ ЖҰБЫ болуы тиіс.
4. "assessmentRubrics" массивінде КЕМІНДЕ 2 БАҒАЛАУ КРИТЕРИЙІ болуы тиіс.

Жауапта тек осы таза JSON-ды ғана қайтарыңыз, ешқандай түсініктемесіз.`;

    const userQuery = `Тақырып: "${topic}". Пән: "${subject}". Сынып: "${grade}". Қосымша мұғалім тілегі: "${additionalNotes || 'Арнайы талаптар жоқ.'}"`;

    try {
      const apiKey = ""; // Canvas ортасында автоматты түрде қойылады
      const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;

      const response = await fetch(apiUrl, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          contents: [{ parts: [{ text: userQuery }] }],
          systemInstruction: {
            parts: [{ text: systemPrompt }]
          },
          generationConfig: {
            responseMimeType: "application/json"
          }
        })
      });

      clearInterval(stepInterval);

      if (!response.ok) {
        throw new Error(`Сервер қатесі: ${response.status}`);
      }

      const result = await response.json();
      const rawText = result.candidates?.[0]?.content?.parts?.[0]?.text;

      if (!rawText) {
        throw new Error("AI-дан ешқандай жауап алынбады.");
      }

      let parsedData;
      try {
        parsedData = JSON.parse(rawText.trim());
      } catch (parseErr) {
        console.error("Парсинг қатесі, тазалауға тырысамыз", rawText);
        
        // Жауаптың ішінен тек JSON бөлігін бөліп алудың қауіпсіз жолы
        let cleanText = rawText.replace(/```json/gi, '').replace(/```/gi, '').trim();
        let startIndex = cleanText.indexOf('{');
        
        if (startIndex === -1) {
          throw new Error("Жауаптан JSON құрылымы табылмады.");
        }
        
        let matched = false;
        // Соңындағы артық таңбаларды (мысалы, қате қосылған '}') алып тастап, артқа қарай тексеру
        for (let i = cleanText.length; i > startIndex; i--) {
          const attempt = cleanText.substring(startIndex, i);
          if (attempt.endsWith('}')) {
            try {
              parsedData = JSON.parse(attempt);
              matched = true;
              break;
            } catch (err) {
              // Қате болса, цикл жалғасады және келесі '}' таңбасына дейін қысқартады
            }
          }
        }
        
        if (!matched) {
          throw new Error("Жауапты JSON форматына айналдыру мүмкін болмады. AI форматын бұзды.");
        }
      }

      if (parsedData.matching) {
        prepareMatchingGame(parsedData.matching);
      }

      if (parsedData.externalResources) {
        setCustomYoutubeQuery(parsedData.externalResources.youtube?.[0]?.queryText || `${topic} қазақша сабақ`);
        setCustomGamesQuery(parsedData.externalResources.games?.[0]?.queryText || `${topic} ойын wordwall`);
        setCustomWebQuery(parsedData.externalResources.websites?.[0]?.queryText || `${topic} bilimland`);
      }

      setGeneratedData(parsedData);
      setActiveTab('plan');

      saveLessonToHistory({
        id: Date.now().toString(),
        topic: topic,
        subject: subject,
        grade: grade,
        timestamp: new Date().toLocaleDateString('kk-KZ', { hour: '2-digit', minute: '2-digit' }),
        data: parsedData
      });

    } catch (err) {
      console.error(err);
      setApiError(err.message || "Сабақ материалдарын жасау кезінде белгісіз қате орын алды. Қайта көріңіз.");
    } finally {
      setIsGenerating(false);
    }
  };

  const generateVisualImage = async () => {
    if (!generatedData || !generatedData.visualPrompt) {
      showNotification('Алдымен сабақ материалдарын жасап алыңыз!', 'error');
      return;
    }

    setIsGeneratingImage(true);
    setImageError(null);
    setGeneratedImageUrl('');

    try {
      // API кілтін қажет етпейтін тегін сурет генерациялау сервисін қолданамыз
      const encodedPrompt = encodeURIComponent(generatedData.visualPrompt + ", highly detailed, educational illustration, 4k");
      const imageUrl = `https://image.pollinations.ai/prompt/${encodedPrompt}?width=1024&height=1024&nologo=true`;

      // Суреттің толық жүктелуін күту (жүктелу анимациясы дұрыс жұмыс істеуі үшін)
      const img = new Image();
      img.onload = () => {
        setGeneratedImageUrl(imageUrl);
        setIsGeneratingImage(false);
        showNotification('Көрнекілік суреті сәтті жасалды!', 'success');
      };
      img.onerror = () => {
        setImageError("Суретті жүктеу кезінде қате орын алды. Желіні тексеріңіз немесе қайта байқап көріңіз.");
        setIsGeneratingImage(false);
      };
      
      img.src = imageUrl;

    } catch (err) {
      console.error(err);
      setImageError(err.message || "Сурет жасауда қате шықты. Жиілік шектеулері немесе желіде ақау болуы мүмкін.");
      setIsGeneratingImage(false);
    }
  };

  const handleSelectMatching = (side, item) => {
    const current = { ...matchingSelections };
    current[side] = item;

    if (matchingSelections[side]?.id === item.id) {
      current[side] = null;
      setMatchingSelections(current);
      return;
    }

    setMatchingSelections(current);

    if (side === 'left' && current.right) {
      checkMatch(item, current.right);
    } else if (side === 'right' && current.left) {
      checkMatch(current.left, item);
    }
  };

  const checkMatch = (leftItem, rightItem) => {
    if (leftItem.id === rightItem.id) {
      setMatchedPairs([...matchedPairs, leftItem.id]);
      setMatchingFeedback({ success: true, message: "Дұрыс сәйкестік! Керемет!" });
      showNotification("Дұрыс сәйкестік!", "success");
    } else {
      setMatchingFeedback({ success: false, message: "Қате! Анықтаманы мұқият оқыңыз." });
      showNotification("Қате! Тағы да көріңіз.", "error");
    }
    
    setTimeout(() => {
      setMatchingSelections({ left: null, right: null });
      setMatchingFeedback(null);
    }, 1200);
  };

  const handleTestOptionClick = (qIdx, option) => {
    if (testChecked) return;
    setTestAnswers({
      ...testAnswers,
      [qIdx]: option
    });
  };

  const calculateCorrectAnswers = () => {
    if (!generatedData || !generatedData.test) return 0;
    let correct = 0;
    generatedData.test.forEach((q, idx) => {
      if (testAnswers[idx] === q.answer) {
        correct++;
      }
    });
    return correct;
  };

  const loadSavedLesson = (lesson) => {
    setGeneratedData(lesson.data);
    setTopic(lesson.topic);
    setGrade(lesson.grade);
    
    const availableSubjects = SUBJECTS_BY_GRADE[lesson.grade] || [];
    if (availableSubjects.includes(lesson.subject)) {
      setSubject(lesson.subject);
    } else {
      setSubject(availableSubjects[0] || lesson.subject);
    }
    
    setGeneratedImageUrl('');
    setTestAnswers({});
    setTestChecked(false);
    setCurrentSlideIndex(0);
    
    if (lesson.data.externalResources) {
      setCustomYoutubeQuery(lesson.data.externalResources.youtube?.[0]?.queryText || `${lesson.topic} қазақша сабақ`);
      setCustomGamesQuery(lesson.data.externalResources.games?.[0]?.queryText || `${lesson.topic} ойын wordwall`);
      setCustomWebQuery(lesson.data.externalResources.websites?.[0]?.queryText || `${lesson.topic} bilimland`);
    }

    if (lesson.data.matching) {
      prepareMatchingGame(lesson.data.matching);
    }
    setActiveTab('plan');
    showNotification('Сабақ тарихтан жүктелді!', 'info');
  };

  const handleAddStudent = (e) => {
    e.preventDefault();
    if (!newStudent.trim()) return;
    if (students.includes(newStudent.trim())) {
      showNotification('Бұл оқушы тізімде бар!', 'error');
      return;
    }
    const updated = [...students, newStudent.trim()];
    setStudents(updated);
    localStorage.setItem('ustaz_students', JSON.stringify(updated));
    setNewStudent("");
    showNotification('Оқушы сәтті қосылды!', 'success');
  };

  const handleRemoveStudent = (index) => {
    const updated = students.filter((_, i) => i !== index);
    setStudents(updated);
    localStorage.setItem('ustaz_students', JSON.stringify(updated));
    showNotification('Оқушы тізімнен өшірілді.', 'info');
  };

  const formatTime = (seconds) => {
    const mins = Math.floor(seconds / 60);
    const secs = seconds % 60;
    return `${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
  };

  const exportToPrint = () => {
    if (!generatedData) return;
    const printWindow = window.open('', '_blank');
    
    const youtubeRes = generatedData?.externalResources?.youtube || [];
    const gamesRes = generatedData?.externalResources?.games || [];
    const websitesRes = generatedData?.externalResources?.websites || [];

    let content = `
      <html>
      <head>
        <title>${generatedData.lessonPlan.title} - Сабақ материалдары</title>
        <style>
          body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; margin: 40px; color: #333; line-height: 1.6; }
          h1 { color: #1e3a8a; border-bottom: 2px solid #1e3a8a; padding-bottom: 10px; }
          h2 { color: #0d9488; margin-top: 30px; border-left: 4px solid #0d9488; padding-left: 10px; }
          h3 { color: #4b5563; }
          table { width: 100%; border-collapse: collapse; margin-top: 20px; margin-bottom: 30px; }
          th, td { border: 1px solid #d1d5db; padding: 12px; text-align: left; vertical-align: top; }
          th { background-color: #f3f4f6; color: #1f2937; }
          .meta-box { background-color: #f0fdfa; border: 1px dashed #0d9488; padding: 15px; border-radius: 8px; margin-bottom: 20px; }
          .slide-card { background: #fafafa; border: 1px solid #e5e7eb; padding: 15px; margin-bottom: 15px; border-radius: 6px; }
          .test-item { margin-bottom: 15px; }
          .bullet { margin-left: 20px; }
          .resource-item { margin-bottom: 10px; padding: 10px; background: #f9fafb; border-radius: 6px; border-left: 3px solid #6366f1; }
        </style>
      </head>
      <body>
        <h1>Ұстаз Көмекшісі - Дайын Сабақ Папкасы</h1>
        <div class="meta-box">
          <p><strong>Тақырыбы:</strong> ${generatedData.lessonPlan.title}</p>
          <p><strong>Пән:</strong> ${generatedData.lessonPlan.subject} | <strong>Сынып:</strong> ${generatedData.lessonPlan.grade}</p>
        </div>

        <h2>I. Сабақтың жоспары (ҚМЖ)</h2>
        <p><strong>Оқу мақсаттары:</strong><br/> ${(generatedData.lessonPlan.learningObjectives || generatedData.lessonPlan.aims || []).join('<br/>')}</p>
        
        <p><strong>Сабақ мақсаттары:</strong></p>
        <ul>
          <li><em>Барлық оқушылар:</em> ${generatedData.lessonPlan.lessonObjectives?.all || ''}</li>
          <li><em>Басым бөлігі:</em> ${generatedData.lessonPlan.lessonObjectives?.most || ''}</li>
          <li><em>Кейбір оқушылар:</em> ${generatedData.lessonPlan.lessonObjectives?.some || ''}</li>
        </ul>

        <p><strong>Бағалау критерийлері:</strong><br/> ${(generatedData.lessonPlan.assessmentCriteria || []).join('<br/>')}</p>
        <p><strong>Құндылықтарды дарыту:</strong> ${generatedData.lessonPlan.values || ''}</p>
        <p><strong>Пәнаралық байланыс:</strong> ${generatedData.lessonPlan.crossCurricular || ''}</p>

        <table>
          <thead>
            <tr>
              <th width="15%">Сабақтың кезеңі / уақыт</th>
              <th width="25%">Педагогтің әрекеті</th>
              <th width="25%">Оқушының әрекеті</th>
              <th width="15%">Бағалау</th>
              <th width="20%">Ресурстар</th>
            </tr>
          </thead>
          <tbody>
            ${generatedData.lessonPlan.stages.map(stage => `
              <tr>
                <td><strong>${stage.name}</strong><br><small>${stage.duration}</small></td>
                <td>${stage.teacherActivity}</td>
                <td>${stage.studentActivity}</td>
                <td><em>${stage.assessment}</em></td>
                <td>${stage.resources || ''}</td>
              </tr>
            `).join('')}
          </tbody>
        </table>

        <h2>II. Бағалау дескрипторлары</h2>
        <ul>
          ${(generatedData.assessmentRubrics || []).map(rubric => `
            <li>
              <strong>${rubric.criteria}</strong>
              <ul>
                ${rubric.descriptors.map(desc => `<li>${desc}</li>`).join('')}
              </ul>
            </li>
          `).join('')}
        </ul>

        <h2>III. Слайдтар құрылымы</h2>
        ${generatedData.slides.map(slide => `
          <div class="slide-card">
            <h3>Слайд №${slide.slideNum}: ${slide.title}</h3>
            <ul>
              ${slide.bullets.map(b => `<li>${b}</li>`).join('')}
            </ul>
            <p><strong>Мұғалімге ескертпе:</strong> <em>${slide.teacherNotes}</em></p>
          </div>
        `).join('')}

        <h2>IV. Тақырыптық Тест сұрақтары</h2>
        ${generatedData.test.map((q, idx) => `
          <div class="test-item">
            <p><strong>${idx + 1}-сұрақ. ${q.question}</strong></p>
            <ul>
              ${q.options.map(opt => `<li>${opt}</li>`).join('')}
            </ul>
            <p style="color: green;"><strong>Дұрыс жауабы:</strong> ${q.answer}</p>
          </div>
        `).join('')}

        <h2>V. Үй тапсырмасы</h2>
        <p>${generatedData.homework}</p>

        <h2>VI. Әдістемелік сурет сипаттамасы</h2>
        <p>${generatedData.visualDescriptionKazakh}</p>

        ${(youtubeRes.length > 0 || gamesRes.length > 0 || websitesRes.length > 0) ? `
          <h2>VII. Пайдалы ресурстар мен сілтемелер</h2>
          ${youtubeRes.length > 0 ? `
            <h3>YouTube бейнематериалдары:</h3>
            ${youtubeRes.map(item => `
              <div class="resource-item">
                <strong>${item.title}</strong><br/>
                <small>Сілтеме: <a href="${item.searchUrl}" target="_blank">${item.searchUrl}</a></small>
              </div>
            `).join('')}
          ` : ''}

          ${gamesRes.length > 0 ? `
            <h3>Интерактивті ойындар мен викториналар:</h3>
            ${gamesRes.map(item => `
              <div class="resource-item">
                <strong>${item.title} (${item.platform})</strong> - ${item.description}<br/>
                <small>Сілтеме: <a href="${item.searchUrl}" target="_blank">${item.searchUrl}</a></small>
              </div>
            `).join('')}
          ` : ''}

          ${websitesRes.length > 0 ? `
            <h3>Ұсынылатын пайдалы веб-сайттар:</h3>
            ${websitesRes.map(item => `
              <div class="resource-item">
                <strong>${item.title} (${item.sourceName})</strong><br/>
                <small>Сілтеме: <a href="${item.url}" target="_blank">${item.url}</a></small>
              </div>
            `).join('')}
          ` : ''}
        ` : ''}

        <script>
          window.onload = function() { window.print(); }
        </script>
      </body>
      </html>
    `;
    printWindow.document.write(content);
    printWindow.document.close();
  };

  const getSlideTheme = (subj) => {
    const s = subj.toLowerCase();
    if (s.includes("қазақ") || s.includes("әдебиет")) {
      return {
        bg: "bg-gradient-to-br from-amber-50 to-orange-100/50 dark:from-slate-900 dark:to-amber-955/20",
        border: "border-amber-200 dark:border-amber-900/50",
        text: "text-amber-900 dark:text-amber-100",
        accent: "bg-amber-600 text-white",
        glow: "shadow-amber-500/10",
        icon: "📜",
        pattern: "KazakhOrnament"
      };
    }
    if (s.includes("математика") || s.includes("алгебра") || s.includes("геометрия") || s.includes("физика")) {
      return {
        bg: "bg-gradient-to-br from-slate-900 to-slate-950",
        border: "border-slate-800",
        text: "text-emerald-400 dark:text-emerald-350",
        accent: "bg-sky-500 text-white",
        glow: "shadow-sky-500/10",
        icon: "📐",
        pattern: "Chalkboard"
      };
    }
    if (s.includes("биология") || s.includes("химия") || s.includes("география")) {
      return {
        bg: "bg-gradient-to-br from-emerald-50 to-teal-100/40 dark:from-slate-900 dark:to-emerald-955/20",
        border: "border-emerald-200 dark:border-emerald-900/40",
        text: "text-emerald-955 dark:text-emerald-200",
        accent: "bg-emerald-600 text-white",
        glow: "shadow-emerald-500/10",
        icon: "🌿",
        pattern: "Lab"
      };
    }
    if (s.includes("тарих")) {
      return {
        bg: "bg-gradient-to-br from-orange-50 to-amber-100/40 dark:from-slate-900 dark:to-orange-955/15",
        border: "border-orange-200 dark:border-orange-900/30",
        text: "text-orange-900 dark:text-orange-100",
        accent: "bg-orange-600 text-white",
        glow: "shadow-orange-500/10",
        icon: "🏺",
        pattern: "History"
      };
    }
    return {
      bg: "bg-gradient-to-br from-indigo-50 to-purple-100/40 dark:from-slate-900 dark:to-indigo-955/15",
      border: "border-indigo-150 dark:border-indigo-900/30",
      text: "text-indigo-955 dark:text-indigo-200",
      accent: "bg-indigo-600 text-white",
      glow: "shadow-indigo-500/10",
      icon: "🏫",
      pattern: "General"
    };
  };

  const startStudentSpin = () => {
    if (isSpinning) return;
    if (students.length === 0) {
      showNotification('Оқушылар тізімі бос! Алдымен оқушыларды қосыңыз.', 'error');
      return;
    }
    
    setIsSpinning(true);
    setSelectedStudent('');
    setGeneratedGroups(null);
    
    let counter = 0;
    const maxCycles = 20;
    const interval = setInterval(() => {
      const randomIndex = Math.floor(Math.random() * students.length);
      setSelectedStudent(students[randomIndex]);
      counter++;
      
      if (counter >= maxCycles) {
        clearInterval(interval);
        setIsSpinning(false);
        showNotification(`Сұраққа жауап береді: ${students[randomIndex]}!`, 'success');
      }
    }, 120);
  };

  const handleMakePairs = () => {
    if (students.length < 2) {
      showNotification('Жұпқа бөлу үшін кемінде 2 оқушы қажет!', 'error');
      return;
    }
    const shuffled = [...students].sort(() => 0.5 - Math.random());
    const pairs = [];
    for (let i = 0; i < shuffled.length; i += 2) {
      pairs.push(shuffled.slice(i, i + 2));
    }
    setSelectedStudent('');
    setGeneratedGroups({ type: 'pairs', data: pairs });
    showNotification('Оқушылар жұптарға бөлінді!', 'success');
  };

  const handleMakeGroups = () => {
    const num = parseInt(numberOfGroups);
    if (students.length < num || num < 2) {
      showNotification(`Топқа бөлу үшін кемінде ${Math.max(2, num)} оқушы қажет!`, 'error');
      return;
    }
    const shuffled = [...students].sort(() => 0.5 - Math.random());
    const groups = Array.from({ length: num }, () => []);
    shuffled.forEach((student, index) => {
      groups[index % num].push(student);
    });
    setSelectedStudent('');
    setGeneratedGroups({ type: 'groups', data: groups });
    showNotification(`Оқушылар ${num} топқа бөлінді!`, 'success');
  };

  return (
    <div className={`min-h-screen transition-colors duration-300 relative overflow-x-hidden ${darkMode ? 'bg-slate-950 text-slate-100' : 'bg-slate-50 text-slate-800'}`}>
      
      {/* Хабарлама билборды */}
      {notification && (
        <div className={`fixed bottom-5 right-5 z-50 flex items-center gap-3 px-5 py-3.5 rounded-2xl shadow-2xl transition-all border transform scale-100 ${
          notification.type === 'success' ? 'bg-emerald-600 text-white border-emerald-700 shadow-emerald-500/10' :
          notification.type === 'error' ? 'bg-rose-600 text-white border-rose-700 shadow-rose-500/10' :
          'bg-indigo-600 text-white border-indigo-700 shadow-indigo-500/10'
        }`}>
          {notification.type === 'success' ? <Check className="w-5 h-5 shrink-0" /> : <AlertCircle className="w-5 h-5 shrink-0" />}
          <span className="font-bold text-sm tracking-tight">{notification.message}</span>
        </div>
      )}

      {/* МҰҒАЛІМНІҢ ЖЕКЕ БЛОКНОТЫ (Persisting Scratchpad Widget) */}
      <div className={`fixed top-24 right-0 z-40 transition-transform duration-300 transform ${isScratchpadOpen ? 'translate-x-0' : 'translate-x-[290px]'}`}>
        <div className="flex items-start">
          <button
            onClick={() => setIsScratchpadOpen(!isScratchpadOpen)}
            className="p-3 bg-gradient-to-r from-indigo-600 to-purple-600 text-white rounded-l-2xl shadow-2xl border-l border-y border-indigo-500/30 flex flex-col items-center gap-1.5 hover:brightness-110"
          >
            <PenTool className="w-5 h-5 animate-pulse" />
            <span className="text-[9px] font-black uppercase tracking-widest [writing-mode:vertical-lr] rotate-180">Блокнот</span>
          </button>
          
          <div className={`w-[290px] h-[380px] p-4 rounded-b-2xl border shadow-2xl flex flex-col justify-between ${
            darkMode ? 'bg-slate-900 border-slate-800 text-slate-100' : 'bg-white border-slate-200 text-slate-850'
          }`}>
            <div className="space-y-1">
              <h4 className="text-xs font-black uppercase tracking-wider text-indigo-500 flex items-center gap-1.5">
                <PenTool className="w-4 h-4" /> Жұмыс дәптері
              </h4>
              <p className="text-[10px] text-slate-400">Сабақ үстінде ескертулер мен белсенді оқушыларды түртіп алыңыз. Деректер браузерде автоматты сақталады.</p>
            </div>
            <textarea
              value={scratchpadText}
              onChange={(e) => handleScratchpadChange(e.target.value)}
              placeholder="Мысалы:&#10;1. Әлібек белсенді қатысты&#10;2. Сабақ соңында дескрипторды тексеру...&#10;3. Үй жұмысын 3-слайдтан беру"
              className={`w-full flex-1 mt-3 p-3 rounded-xl border text-xs focus:outline-none focus:ring-1 focus:ring-indigo-500 resize-none font-mono ${
                darkMode ? 'bg-slate-950 border-slate-800 text-slate-200' : 'bg-slate-50 border-slate-100 text-slate-700'
              }`}
            />
            <button
              onClick={() => {
                handleScratchpadChange('');
                showNotification('Блокнот тазартылды', 'info');
              }}
              className="mt-2 text-center text-[10px] font-bold text-rose-500 hover:underline"
            >
              Мәтінді тазарту
            </button>
          </div>
        </div>
      </div>

      {/* Жоғарғы Бас Мега Навигация */}
      <header className={`sticky top-0 z-40 backdrop-blur-md border-b transition-colors ${darkMode ? 'bg-slate-950/90 border-slate-900' : 'bg-white/90 border-slate-100'}`}>
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex flex-col sm:flex-row justify-between items-center gap-4">
          <div className="flex items-center gap-3.5">
            <div className="bg-gradient-to-tr from-emerald-500 to-indigo-600 p-2.5 rounded-2xl shadow-lg text-white transform hover:rotate-3 transition-transform">
              <BookOpen className="w-7 h-7" />
            </div>
            <div>
              <h1 className="text-xl sm:text-2xl font-black bg-gradient-to-r from-teal-500 via-emerald-500 to-indigo-600 bg-clip-text text-transparent">
                ҰстазКөмекшісі <span className="text-xs bg-indigo-100 dark:bg-indigo-950/50 text-indigo-700 dark:text-indigo-300 px-2.5 py-0.5 rounded-full font-extrabold">ЖИ Платформасы</span>
              </h1>
              <p className="text-xs text-slate-400 font-medium tracking-tight">ҚР орта білім стандартына сай озық педагогикалық серіктесіңіз</p>
            </div>
          </div>
          
          <div className="flex items-center gap-3">
            <button
              onClick={() => setDarkMode(!darkMode)}
              className={`p-2.5 rounded-xl border transition-all ${darkMode ? 'bg-slate-900 border-slate-800 hover:bg-slate-800 text-amber-400' : 'bg-slate-100 border-slate-200 hover:bg-slate-200 text-slate-600'}`}
              title="Режимді ауыстыру"
            >
              {darkMode ? <Sun className="w-5 h-5" /> : <Moon className="w-5 h-5" />}
            </button>
            <span className="text-xs font-bold px-3.5 py-1.5 rounded-xl bg-gradient-to-r from-emerald-500/10 to-teal-500/10 text-emerald-600 border border-emerald-500/20 shadow-sm">
              Бағдарлама: Орта білім
            </span>
          </div>
        </div>
      </header>

      {/* Негізгі Контент Контейнері */}
      <main className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        
        {/* Көмекші Банер және Кіріспе */}
        <div className="mb-8 p-6 rounded-3xl bg-gradient-to-r from-emerald-500/10 via-indigo-500/5 to-purple-500/10 border border-emerald-500/20 shadow-xl relative overflow-hidden group">
          <div className="absolute -right-10 -bottom-10 w-40 h-40 bg-indigo-500/10 rounded-full blur-3xl group-hover:scale-125 transition-transform"></div>
          <div className="flex flex-col md:flex-row md:items-center justify-between gap-6 relative z-10">
            <div className="space-y-2">
              <span className="inline-flex items-center gap-1.5 px-3 py-1 rounded-xl text-xs font-bold bg-indigo-100 dark:bg-indigo-950 text-indigo-800 dark:text-indigo-300 shadow-sm">
                <Sparkles className="w-3.5 h-3.5 text-indigo-600" /> Жаңа Жаңарту: Дескрипторлар, Инклюзивті қолдау мен Толық Слайд Театры!
              </span>
              <h2 className="text-2xl font-extrabold tracking-tight">Жылдам әрі Сапалы: Дайын Сабақ Жоспарын Жүктеңіз</h2>
              <p className="text-sm text-slate-500 dark:text-slate-400 max-w-3xl leading-relaxed">
                Сабақтың тақырыбын жазып, бірнеше секунд ішінде толық ҚМЖ (қысқа мерзімді жоспар), бағалау дескрипторларын, проекциялық слайдтар театрын, интерактивті тесттер мен сыртқы смарт ресурстар жинағын алыңыз.
              </p>
            </div>
          </div>
        </div>

        {/* Екі бағанды орналасу */}
        <div className="grid grid-cols-1 lg:grid-cols-12 gap-8">
          
          {/* СОЛ ЖАҚ БАҒАН: Конструктор (4/12 бөлік) */}
          <div className="lg:col-span-4 space-y-6">
            
            {/* Сабақ Параметрлері Карточкасы */}
            <div className={`p-6 rounded-3xl border shadow-xl transition-all ${darkMode ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200'}`}>
              <div className="flex items-center gap-2.5 mb-6">
                <div className="p-2 rounded-xl bg-gradient-to-tr from-emerald-500 to-indigo-600 text-white shadow-md">
                  <Sparkles className="w-5 h-5" />
                </div>
                <h3 className="text-lg font-bold">Сабақ құрастырушы</h3>
              </div>

              <div className="space-y-5">
                {/* 1. Тақырып */}
            <div>
              <label className="block text-xs font-bold uppercase tracking-wider mb-2 text-slate-400">Сабақтың тақырыбы</label>
              <input
                type="text"
                value={topic}
                onChange={(e) => setTopic(e.target.value)}
                placeholder="Мысалы: Фотосинтез процесі"
                className={`w-full px-4 py-3 rounded-xl border focus:outline-none focus:ring-2 focus:ring-emerald-500 transition-all text-sm ${
                  darkMode ? 'bg-slate-950 border-slate-800 text-slate-100 placeholder-slate-600' : 'bg-slate-50 border-slate-100 text-slate-800 placeholder-slate-400'
                }`}
              />
            </div>

            {/* 2. Пән мен Сынып қатар */}
            <div className="grid grid-cols-2 gap-4">
              <div>
                <label className="block text-xs font-bold uppercase tracking-wider mb-2 text-slate-400">Сынып</label>
                <select
                  value={grade}
                  onChange={(e) => {
                    const newGrade = e.target.value;
                    setGrade(newGrade);
                    const availableSubjects = SUBJECTS_BY_GRADE[newGrade];
                    if (!availableSubjects.includes(subject)) {
                      setSubject(availableSubjects[0]);
                    }
                  }}
                  className={`w-full px-3 py-3 rounded-xl border focus:outline-none focus:ring-2 focus:ring-emerald-500 transition-all text-sm ${
                    darkMode ? 'bg-slate-950 border-slate-800 text-slate-100' : 'bg-slate-50 border-slate-100 text-slate-800'
                  }`}
                >
                  {GRADES.map((gr, idx) => (
                    <option key={idx} value={gr}>{gr}</option>
                  ))}
                </select>
              </div>

              <div>
                <label className="block text-xs font-bold uppercase tracking-wider mb-2 text-slate-400">Пән таңдау</label>
                <select
                  value={subject}
                  onChange={(e) => setSubject(e.target.value)}
                  className={`w-full px-3 py-3 rounded-xl border focus:outline-none focus:ring-2 focus:ring-emerald-500 transition-all text-sm ${
                    darkMode ? 'bg-slate-950 border-slate-800 text-slate-100' : 'bg-slate-50 border-slate-100 text-slate-800'
                  }`}
                >
                  {(SUBJECTS_BY_GRADE[grade] || []).map((sub, idx) => (
                    <option key={idx} value={sub}>{sub}</option>
                  ))}
                </select>
              </div>
            </div>

            {/* 3. Қосымша мұғалім талаптары */}
            <div>
              <label className="block text-xs font-bold uppercase tracking-wider mb-2 text-slate-400">Қосымша талаптар (міндетті емес)</label>
                  <textarea
                    value={additionalNotes}
                    onChange={(e) => setAdditionalNotes(e.target.value)}
                    placeholder="Ерекше әдістер, бағалау критерийлері немесе назар аударатын тұстарды жазыңыз..."
                    rows={3}
                    className={`w-full px-4 py-3 rounded-xl border focus:outline-none focus:ring-2 focus:ring-emerald-500 transition-all text-sm resize-none ${
                      darkMode ? 'bg-slate-950 border-slate-800 text-slate-100 placeholder-slate-600' : 'bg-slate-50 border-slate-100 text-slate-800 placeholder-slate-400'
                    }`}
                  />
                </div>

                {/* Генерациялау Батырмасы */}
                <button
                  onClick={generateLessonMaterial}
                  disabled={isGenerating}
                  className={`w-full py-4 px-6 rounded-xl font-bold text-white shadow-lg transition-all flex items-center justify-center gap-2 ${
                    isGenerating 
                      ? 'bg-slate-600 cursor-not-allowed' 
                      : 'bg-gradient-to-r from-emerald-500 to-indigo-600 hover:from-emerald-600 hover:to-indigo-700 hover:shadow-emerald-500/20 hover:-translate-y-0.5'
                  }`}
                >
                  {isGenerating ? (
                    <>
                      <div className="w-5 h-5 border-t-2 border-r-2 border-white rounded-full animate-spin"></div>
                      <span className="text-sm">Дайындауда...</span>
                    </>
                  ) : (
                    <>
                      <Sparkles className="w-5 h-5" />
                      <span>Сабақ материалдарын жасау</span>
                    </>
                  )}
                </button>

                {apiError && (
                  <div className="p-4 rounded-xl bg-rose-50 border border-rose-100 text-rose-600 flex items-start gap-2.5">
                    <AlertCircle className="w-5 h-5 shrink-0 mt-0.5" />
                    <p className="text-xs font-medium leading-relaxed">{apiError}</p>
                  </div>
                )}
              </div>
            </div>

            {/* ТЕЗ СЫНАП КӨРУГЕ АРНАЛҒАН ЖЫЛДАМ ШАБЛОНДАР */}
            <div className={`p-6 rounded-3xl border shadow-md ${darkMode ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200'}`}>
              <h4 className="text-sm font-bold text-slate-400 uppercase tracking-wider mb-4">Жылдам тақырып үлгілері</h4>
              <div className="space-y-2">
                {TEMPLATES.map((tpl, idx) => (
                  <button
                    key={idx}
                    onClick={() => applyTemplate(tpl)}
                    className={`w-full text-left p-3 rounded-xl text-xs font-medium border transition-all flex items-center justify-between group ${
                      darkMode ? 'bg-slate-950 border-slate-800 hover:bg-slate-800 text-slate-300' : 'bg-slate-50 border-slate-100 hover:bg-slate-100 text-slate-600'
                    }`}
                  >
                    <div className="truncate pr-2">
                      <span className="font-semibold block text-slate-700 dark:text-slate-200 truncate">{tpl.topic}</span>
                      <span className="text-[10px] text-slate-400">{tpl.subject} • {tpl.grade}</span>
                    </div>
                    <ChevronRight className="w-4 h-4 text-slate-400 group-hover:translate-x-1 transition-transform shrink-0" />
                  </button>
                ))}
              </div>
            </div>

            {/* ТАРИХ ПЕН САҚТАЛҒАНДАР */}
            <div className={`p-6 rounded-3xl border shadow-md ${darkMode ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200'}`}>
              <div className="flex justify-between items-center mb-4">
                <div className="flex items-center gap-2">
                  <Clock className="w-4.5 h-4.5 text-indigo-500" />
                  <h4 className="text-sm font-bold text-slate-400 uppercase tracking-wider">Соңғы сақталған сабақтар</h4>
                </div>
                <span className="text-xs font-bold text-slate-400">({savedLessons.length})</span>
              </div>

              {savedLessons.length === 0 ? (
                <div className="text-center py-6 border border-dashed rounded-2xl border-slate-200 dark:border-slate-700">
                  <Bookmark className="w-8 h-8 text-slate-300 mx-auto mb-2" />
                  <p className="text-xs text-slate-400">Әлі сабақтар сақталмады. Жасалған сабақтар осында автоматты түрде жиналады.</p>
                </div>
              ) : (
                <div className="space-y-2 max-h-64 overflow-y-auto pr-1">
                  {savedLessons.map((lesson) => (
                    <div
                      key={lesson.id}
                      onClick={() => loadSavedLesson(lesson)}
                      className={`p-3 rounded-xl border transition-all flex items-center justify-between cursor-pointer group ${
                        darkMode ? 'bg-slate-950 border-slate-800 hover:bg-slate-805' : 'bg-slate-50 border-slate-100 hover:bg-slate-100'
                      }`}
                    >
                      <div className="truncate pr-2">
                        <span className="font-semibold text-xs block text-slate-700 dark:text-slate-200 truncate">{lesson.topic}</span>
                        <span className="text-[10px] text-slate-400">{lesson.subject} • {lesson.grade} • {lesson.timestamp}</span>
                      </div>
                      <button
                        onClick={(e) => deleteLessonFromHistory(lesson.id, e)}
                        className="p-1.5 rounded-lg text-slate-400 hover:text-rose-500 hover:bg-rose-50 dark:hover:bg-rose-950/30 transition-colors opacity-0 group-hover:opacity-100 focus:opacity-100"
                        title="Өшіру"
                      >
                        <Trash2 className="w-3.5 h-3.5" />
                      </button>
                    </div>
                  ))}
                </div>
              )}
            </div>

          </div>

          {/* ОҢ ЖАҚ БАҒАН: Материалдарды көрсету экраны (8/12 бөлік) */}
          <div className="lg:col-span-8">
            
            {/* Генерациялау барысындағы анимациялық жүктелу экраны */}
            {isGenerating && (
              <div className={`p-12 rounded-3xl border shadow-2xl flex flex-col items-center justify-center text-center space-y-6 min-h-[500px] ${
                darkMode ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200'
              }`}>
                <div className="relative">
                  <div className="w-20 h-20 border-4 border-emerald-500/20 rounded-full animate-ping absolute top-0 left-0"></div>
                  <div className="w-20 h-20 border-4 border-indigo-500 border-t-transparent border-r-transparent rounded-full animate-spin"></div>
                  <div className="w-12 h-12 bg-gradient-to-tr from-emerald-500 to-indigo-600 rounded-full absolute top-4 left-4 flex items-center justify-center text-white">
                    <Sparkles className="w-6 h-6 animate-pulse" />
                  </div>
                </div>
                
                <div className="space-y-2">
                  <h3 className="text-xl font-black text-indigo-500 animate-pulse">{generationStep}</h3>
                  <p className="text-sm text-slate-400 max-w-md">
                    Жасанды интеллект сіздің тақырыбыңыз бойынша оқу жоспарын, тесттерді, интерактивті тапсырмаларды және бағалау өлшемдерін жинақтап жатыр. Күте тұрыңыз...
                  </p>
                </div>

                <div className="w-full max-w-xs bg-slate-200 dark:bg-slate-700 h-2 rounded-full overflow-hidden">
                  <div className="bg-gradient-to-r from-emerald-500 to-indigo-500 h-full w-4/5 rounded-full animate-pulse"></div>
                </div>
              </div>
            )}

            {/* Бос экран - Мәлімет дайын емес кездегі күй */}
            {!isGenerating && !generatedData && (
              <div className={`p-12 rounded-3xl border shadow-xl flex flex-col items-center justify-center text-center space-y-6 min-h-[500px] border-dashed ${
                darkMode ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200'
              }`}>
                <div className="bg-gradient-to-tr from-emerald-100 to-indigo-150 p-6 rounded-full text-indigo-500 dark:text-indigo-400">
                  <BookOpen className="w-16 h-16 animate-pulse" />
                </div>
                <div className="space-y-2 max-w-md">
                  <h3 className="text-2xl font-black tracking-tight">Дайын материалдар мұнда пайда болады</h3>
                  <p className="text-sm text-slate-400">
                    Сол жақтағы конструкторға сабақтың тақырыбын енгізіңіз (мысалы, белгілі бір биологиялық не физикалық құбылыс, әдеби шығарма немесе математикалық формула) және <strong>"Сабақ материалдарын жасау"</strong> батырмасын басыңыз.
                  </p>
                </div>
                <div className="flex items-center gap-2.5 p-3.5 bg-indigo-50 dark:bg-slate-950 text-indigo-600 dark:text-indigo-400 text-xs font-semibold rounded-2xl border border-indigo-500/10">
                  <Info className="w-4.5 h-4.5 shrink-0" />
                  <span>Қазақша сапалы контент генерациялау үшін оңтайлы баптаулар қосылған.</span>
                </div>
              </div>
            )}

            {/* ГЕНЕРАЦИЯЛАНҒАН МӘЛІМЕТТЕРДІ КӨРСЕТУ ТАҚТАСЫ */}
            {!isGenerating && generatedData && (
              <div className="space-y-6">
                
                {/* Тақырып және Сақтау/Баспа панелі */}
                <div className={`p-6 rounded-3xl border shadow-lg flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 ${
                  darkMode ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200'
                }`}>
                  <div>
                    <span className="text-xs font-bold text-emerald-500 uppercase tracking-widest">{generatedData.lessonPlan.subject} • {generatedData.lessonPlan.grade}</span>
                    <h2 className="text-xl sm:text-2xl font-black tracking-tight mt-1">{generatedData.lessonPlan.title}</h2>
                  </div>
                  
                  <div className="flex flex-wrap items-center gap-2 w-full sm:w-auto">
                    <button
                      onClick={exportToPrint}
                      className="flex-1 sm:flex-none py-2 px-4 rounded-xl border border-indigo-500/30 text-indigo-500 font-bold text-xs hover:bg-indigo-50 dark:hover:bg-indigo-950/20 transition-all flex items-center justify-center gap-1.5"
                    >
                      <Download className="w-4 h-4" />
                      <span>Баспаға шығару / PDF</span>
                    </button>
                    <button
                      onClick={() => copyToClipboard(JSON.stringify(generatedData, null, 2))}
                      className="flex-1 sm:flex-none py-2 px-4 rounded-xl bg-slate-100 dark:bg-slate-800 text-slate-700 dark:text-slate-200 font-bold text-xs hover:bg-slate-250 transition-all flex items-center justify-center gap-1.5"
                    >
                      <Clipboard className="w-4 h-4" />
                      <span>Кодын Көшіру</span>
                    </button>
                  </div>
                </div>

                {/* Қойындыларды ауыстыру навигациясы (Tabs) */}
                <div className="flex overflow-x-auto pb-2 gap-2 scrollbar-none">
                  {[
                    { id: 'plan', label: 'Сабақ Жоспары (ҚМЖ)', icon: FileText },
                    { id: 'slides', label: 'Слайдтар Театры', icon: Layers },
                    { id: 'test', label: 'Тест Тапсырмасы', icon: CheckSquare },
                    { id: 'matching', label: 'Сәйкестендіру', icon: Shuffle },
                    { id: 'resources', label: 'Ресурстар', icon: Link2 },
                    { id: 'assessment', label: 'Бағалау өлшемдері', icon: Award },
                    { id: 'visuals', label: 'Көрнекіліктер (Imagen)', icon: ImageIcon },
                    { id: 'tools', label: 'Сынып Құралдары', icon: Users },
                    { id: 'tasks', label: 'Тапсырмалар', icon: PenTool },
                    { id: 'homework', label: 'Үй тапсырмасы', icon: Home }
                  ].map((tab) => {
                    const Icon = tab.icon;
                    const isActive = activeTab === tab.id;
                    return (
                      <button
                        key={tab.id}
                        onClick={() => setActiveTab(tab.id)}
                        className={`py-3 px-4 rounded-2xl font-bold text-xs transition-all flex items-center gap-2 shrink-0 ${
                          isActive 
                            ? 'bg-gradient-to-r from-emerald-500 to-indigo-600 text-white shadow-lg transform scale-102' 
                            : 'bg-white dark:bg-slate-900 text-slate-600 dark:text-slate-300 border border-slate-200 dark:border-slate-800 hover:bg-slate-50'
                        }`}
                      >
                        <Icon className={`w-4.5 h-4.5 ${isActive ? 'text-white' : ''}`} />
                        <span>{tab.label}</span>
                      </button>
                    );
                  })}
                </div>

                {/* ҚОЙЫНДЫ МАЗМҰНДАРЫ */}
                <div className={`p-6 sm:p-8 rounded-3xl border shadow-xl transition-all ${
                  darkMode ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200'
                }`}>
                  
                  {/* ТАВ 1: САБАҚ ЖОСПАРЫ (ҚМЖ үлгісінде) */}
                  {activeTab === 'plan' && (
                    <div className="space-y-6 animate-fadeIn">
                      <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center border-b pb-4 gap-2">
                        <h3 className="text-lg font-bold flex items-center gap-2 text-teal-600 dark:text-teal-400">
                          <FileText className="w-5 h-5" /> Сабақтың жоспар-конспектісі (ҚМЖ)
                        </h3>
                        <button 
                          onClick={() => copyToClipboard(JSON.stringify(generatedData.lessonPlan, null, 2))}
                          className="text-xs text-slate-400 hover:text-slate-600 dark:hover:text-slate-200 flex items-center gap-1"
                        >
                          <Clipboard className="w-3.5 h-3.5" /> Жоспарды көшіру
                        </button>
                      </div>

                      <div className="grid grid-cols-1 md:grid-cols-2 gap-4 bg-slate-50 dark:bg-slate-950 p-5 rounded-2xl border border-slate-200 dark:border-slate-850">
                        <div className="space-y-4">
                          <div>
                            <h4 className="text-xs font-bold uppercase text-indigo-500 mb-2">Оқу мақсаттары (ОМ)</h4>
                            <ul className="space-y-1.5 text-xs sm:text-sm font-medium">
                              {(generatedData.lessonPlan.learningObjectives || generatedData.lessonPlan.aims || []).map((aim, idx) => (
                                <li key={idx} className="flex items-start gap-2 text-slate-600 dark:text-slate-300">
                                  <span className="bg-indigo-100 dark:bg-indigo-950 text-indigo-700 dark:text-indigo-300 w-5 h-5 rounded-full flex items-center justify-center shrink-0 font-bold text-[10px] mt-0.5">✓</span>
                                  <span className="leading-relaxed">{aim}</span>
                                </li>
                              ))}
                            </ul>
                          </div>
                          <div>
                            <h4 className="text-xs font-bold uppercase text-emerald-500 mb-2">Сабақ мақсаттары</h4>
                            <ul className="space-y-2 text-xs sm:text-sm text-slate-600 dark:text-slate-300 font-medium bg-white dark:bg-slate-900 p-3 rounded-xl border border-slate-200 dark:border-slate-800">
                              <li><strong className="text-slate-700 dark:text-slate-200">Барлық оқушылар:</strong> {generatedData.lessonPlan.lessonObjectives?.all || ''}</li>
                              <li><strong className="text-slate-700 dark:text-slate-200">Басым бөлігі:</strong> {generatedData.lessonPlan.lessonObjectives?.most || ''}</li>
                              <li><strong className="text-slate-700 dark:text-slate-200">Кейбір оқушылар:</strong> {generatedData.lessonPlan.lessonObjectives?.some || ''}</li>
                            </ul>
                          </div>
                        </div>
                        <div className="space-y-4">
                          <div>
                            <h4 className="text-xs font-bold uppercase text-amber-500 mb-2">Бағалау критерийлері</h4>
                            <ul className="space-y-1.5 text-xs sm:text-sm font-medium list-disc list-inside text-slate-600 dark:text-slate-300 pl-2">
                              {(generatedData.lessonPlan.assessmentCriteria || []).map((crit, idx) => (
                                <li key={idx}>{crit}</li>
                              ))}
                            </ul>
                          </div>
                          <div className="grid grid-cols-2 gap-4 pt-2">
                            <div>
                              <h4 className="text-xs font-bold uppercase text-purple-500 mb-1">Құндылықтар</h4>
                              <p className="text-xs sm:text-sm text-slate-600 dark:text-slate-300 font-medium bg-white dark:bg-slate-900 p-2.5 rounded-xl border border-slate-200 dark:border-slate-800 h-full">
                                {generatedData.lessonPlan.values || ''}
                              </p>
                            </div>
                            <div>
                              <h4 className="text-xs font-bold uppercase text-sky-500 mb-1">Пәнаралық байланыс</h4>
                              <p className="text-xs sm:text-sm text-slate-600 dark:text-slate-300 font-medium bg-white dark:bg-slate-900 p-2.5 rounded-xl border border-slate-200 dark:border-slate-800 h-full">
                                {generatedData.lessonPlan.crossCurricular || ''}
                              </p>
                            </div>
                          </div>
                        </div>
                      </div>

                      {/* Түпкілікті Сабақ Хронологиясы */}
                      <div className="space-y-4">
                        <h4 className="text-sm font-bold text-slate-400 uppercase tracking-wider">Сабақ барысы</h4>
                        <div className="space-y-4 overflow-x-auto rounded-xl border border-slate-200 dark:border-slate-700">
                          <table className="w-full text-left text-sm border-collapse min-w-[800px]">
                            <thead>
                              <tr className="bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 text-[11px] font-black uppercase tracking-wider border-b border-slate-200 dark:border-slate-700">
                                <th className="p-4 w-1/6 border-r border-slate-200 dark:border-slate-700">Кезең/Уақыт</th>
                                <th className="p-4 w-1/4 border-r border-slate-200 dark:border-slate-700">Педагогтің әрекеті</th>
                                <th className="p-4 w-1/4 border-r border-slate-200 dark:border-slate-700">Оқушының әрекеті</th>
                                <th className="p-4 w-1/6 border-r border-slate-200 dark:border-slate-700">Бағалау</th>
                                <th className="p-4 w-1/6">Ресурстар</th>
                              </tr>
                            </thead>
                            <tbody>
                              {generatedData.lessonPlan.stages.map((stage, idx) => (
                                <tr key={idx} className="border-b last:border-b-0 border-slate-200 dark:border-slate-700 hover:bg-slate-50 dark:hover:bg-slate-900/50 transition-colors">
                                  <td className="p-4 align-top border-r border-slate-200 dark:border-slate-700">
                                    <strong className="block text-slate-800 dark:text-slate-200 leading-tight">{stage.name}</strong>
                                    <span className="text-[10px] font-bold text-teal-700 dark:text-teal-300 bg-teal-100 dark:bg-teal-950 px-2 py-1 rounded-md mt-2 inline-block shadow-sm">Уақыты: {stage.duration}</span>
                                  </td>
                                  <td className="p-4 align-top border-r border-slate-200 dark:border-slate-700 text-slate-600 dark:text-slate-300 whitespace-pre-wrap leading-relaxed">{stage.teacherActivity}</td>
                                  <td className="p-4 align-top border-r border-slate-200 dark:border-slate-700 text-slate-600 dark:text-slate-300 whitespace-pre-wrap leading-relaxed">{stage.studentActivity}</td>
                                  <td className="p-4 align-top border-r border-slate-200 dark:border-slate-700 text-slate-600 dark:text-slate-300 italic whitespace-pre-wrap leading-relaxed bg-amber-50/30 dark:bg-amber-950/10">{stage.assessment}</td>
                                  <td className="p-4 align-top text-indigo-600 dark:text-indigo-400 font-medium whitespace-pre-wrap leading-relaxed bg-indigo-50/30 dark:bg-indigo-950/10">{stage.resources || ''}</td>
                                </tr>
                              ))}
                            </tbody>
                          </table>
                        </div>
                      </div>
                    </div>
                  )}

                  {/* ТАВ 2: СЛАЙДТАРДЫҢ МӘТІНДЕРІ */}
                  {activeTab === 'slides' && (
                    <div className="space-y-6 animate-fadeIn">
                      
                      <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center border-b pb-4 gap-2">
                        <div>
                          <h3 className="text-lg font-bold flex items-center gap-2 text-indigo-600 dark:text-indigo-400">
                            <Layers className="w-5 h-5" /> Интерактивті Слайд Театры
                          </h3>
                          <p className="text-xs text-slate-400 mt-0.5">Сабақта тікелей көрсетуге немесе тақтаға шығаруға арналған көрнекілік презентация</p>
                        </div>
                        <div className="flex items-center gap-2">
                          <button
                            onClick={() => speakText(`${generatedData.slides[currentSlideIndex].title}. ${generatedData.slides[currentSlideIndex].bullets.join('. ')}`)}
                            className="p-2 bg-indigo-50 hover:bg-indigo-100 text-indigo-600 rounded-xl dark:bg-slate-800 dark:text-indigo-400 transition-colors"
                            title="Дауыстап оқу (Инклюзивті қолдау)"
                          >
                            <Volume2 className="w-5 h-5" />
                          </button>
                          <button
                            onClick={() => setIsSlideFullscreen(true)}
                            className="p-2 bg-indigo-50 hover:bg-indigo-100 text-indigo-600 rounded-xl dark:bg-slate-800 dark:text-indigo-400 transition-colors"
                            title="Толық экранға шығару"
                          >
                            <Maximize2 className="w-5 h-5" />
                          </button>
                          <span className="text-xs bg-indigo-100 dark:bg-indigo-950 text-indigo-700 dark:text-indigo-300 px-3 py-1.5 rounded-full font-bold shrink-0">
                            Слайд {currentSlideIndex + 1} / {generatedData.slides.length}
                          </span>
                        </div>
                      </div>

                      {(() => {
                        const currentSlide = generatedData.slides[currentSlideIndex];
                        const theme = getSlideTheme(generatedData.lessonPlan.subject);
                        
                        return (
                          <div className="space-y-4">
                            <div className={`relative rounded-3xl p-6 sm:p-10 min-h-[380px] sm:min-h-[440px] border shadow-2xl transition-all duration-300 flex flex-col justify-between ${theme.bg} ${theme.border} ${theme.glow}`}>
                              
                              <div className="flex justify-between items-center text-[11px] font-extrabold tracking-widest uppercase text-slate-400 opacity-80 border-b pb-3 border-dashed border-slate-500/25">
                                <span className="flex items-center gap-1">
                                  <span>{theme.icon}</span>
                                  <span>{generatedData.lessonPlan.subject} • {generatedData.lessonPlan.grade}</span>
                                </span>
                                <span>СЛАЙД #{currentSlide.slideNum}</span>
                              </div>

                              <div className="grid grid-cols-1 md:grid-cols-12 gap-8 items-center py-6">
                                <div className="md:col-span-5 flex flex-col items-center justify-center">
                                  <div className="relative w-full max-w-[220px] aspect-square rounded-2xl bg-white/10 dark:bg-black/20 border border-slate-400/20 shadow-inner flex flex-col items-center justify-center p-6 text-center group overflow-hidden">
                                    {theme.pattern === "Chalkboard" && (
                                      <div className="absolute inset-0 flex items-center justify-center text-slate-500/10 font-mono text-[9px] pointer-events-none p-2 select-none overflow-hidden">
                                        E = mc² <br /> f(x) = ax² + bx + c <br /> π ≈ 3.14159 <br /> Δx · Δp ≥ ℏ/2
                                      </div>
                                    )}
                                    {theme.pattern === "KazakhOrnament" && (
                                      <div className="absolute inset-0 opacity-5 flex items-center justify-center pointer-events-none select-none">
                                        <div className="w-32 h-32 border-8 border-amber-900 rounded-full flex items-center justify-center">
                                          <div className="w-16 h-16 border-4 border-amber-900 rotate-45"></div>
                                        </div>
                                      </div>
                                    )}
                                    <div className="text-6xl mb-3 filter drop-shadow-md">{theme.icon}</div>
                                    <span className="text-xs font-extrabold tracking-tight text-slate-400 block truncate max-w-full">{currentSlide.title}</span>
                                    <div className="absolute bottom-2 text-[9px] font-bold text-slate-500 tracking-wider">КӨРНЕКІ ИЛЛЮСТРАЦИЯ</div>
                                  </div>
                                </div>

                                <div className="md:col-span-7 space-y-5">
                                  <h4 className="text-xl sm:text-2xl font-black tracking-tight leading-tight text-slate-800 dark:text-white">{currentSlide.title}</h4>
                                  <ul className="space-y-3">
                                    {currentSlide.bullets.map((bullet, bIdx) => (
                                      <li key={bIdx} className="flex items-start gap-3 text-xs sm:text-sm text-slate-700 dark:text-slate-200 font-semibold leading-relaxed">
                                        <span className="bg-emerald-500 text-white w-5 h-5 rounded-full flex items-center justify-center shrink-0 font-bold text-[10px] mt-0.5">✓</span>
                                        <span>{bullet}</span>
                                      </li>
                                    ))}
                                  </ul>
                                </div>
                              </div>

                              <div className="flex justify-between items-center pt-4 border-t border-dashed border-slate-500/25">
                                <button
                                  onClick={() => setCurrentSlideIndex(prev => Math.max(0, prev - 1))}
                                  disabled={currentSlideIndex === 0}
                                  className={`py-2 px-4 rounded-xl font-bold text-xs transition-all ${
                                    currentSlideIndex === 0 
                                      ? 'bg-slate-500/15 text-slate-500 cursor-not-allowed' 
                                      : 'bg-white/10 hover:bg-white/20 text-slate-350 border border-slate-500/30'
                                  }`}
                                >
                                  ← Артқа
                                </button>
                                <div className="flex gap-1.5">
                                  {generatedData.slides.map((_, dotIdx) => (
                                    <button
                                      key={dotIdx}
                                      onClick={() => setCurrentSlideIndex(dotIdx)}
                                      className={`h-2.5 rounded-full transition-all duration-300 ${
                                        dotIdx === currentSlideIndex ? 'w-6 bg-emerald-500' : 'w-2.5 bg-slate-500/40'
                                      }`}
                                    ></button>
                                  ))}
                                </div>
                                <button
                                  onClick={() => setCurrentSlideIndex(prev => Math.min(generatedData.slides.length - 1, prev + 1))}
                                  disabled={currentSlideIndex === generatedData.slides.length - 1}
                                  className={`py-2 px-4 rounded-xl font-bold text-xs transition-all ${
                                    currentSlideIndex === generatedData.slides.length - 1 
                                      ? 'bg-slate-500/15 text-slate-500 cursor-not-allowed' 
                                      : 'bg-white/10 hover:bg-white/20 text-slate-350 border border-slate-500/30'
                                  }`}
                                >
                                  Келесі →
                                </button>
                              </div>
                            </div>

                            <div className="p-5 rounded-2xl bg-amber-500/5 border border-amber-500/20 shadow-sm relative overflow-hidden">
                              <div className="absolute top-0 left-0 w-1.5 h-full bg-amber-500"></div>
                              <div className="flex items-center gap-2 mb-2">
                                <span className="text-sm">💡</span>
                                <span className="text-xs font-extrabold text-amber-600 uppercase tracking-widest">Мұғалімге әдістемелік нұсқаулық (Notes)</span>
                              </div>
                              <p className="text-xs sm:text-sm text-slate-600 dark:text-slate-300 italic font-medium leading-relaxed pl-3">"{currentSlide.teacherNotes}"</p>
                            </div>
                          </div>
                        );
                      })()}
                    </div>
                  )}

                  {/* ТАВ 3: ТАҚЫРЫПТЫҚ ТЕСТ ТАПСЫРМАЛАРЫ */}
                  {activeTab === 'test' && (
                    <div className="space-y-6 animate-fadeIn">
                      <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center border-b pb-4 gap-2">
                        <div>
                          <h3 className="text-lg font-bold flex items-center gap-2 text-emerald-600 dark:text-emerald-400">
                            <CheckSquare className="w-5 h-5" /> Тақырыптық Тест сұрақтары
                          </h3>
                          <p className="text-xs text-slate-400 mt-1">Оқушылардан жауап алу немесе интерактивті тақтада ойнату үшін таптырмас құрал</p>
                        </div>
                        <button 
                          onClick={() => {
                            setTestAnswers({});
                            setTestChecked(false);
                            showNotification('Тест жауаптары тазартылды!', 'info');
                          }}
                          className="text-xs text-slate-500 hover:text-slate-700 dark:hover:text-slate-300 font-bold"
                        >
                          Қайта тапсыру
                        </button>
                      </div>

                      <div className="space-y-6">
                        {generatedData.test.map((q, qIdx) => {
                          const userSelected = testAnswers[qIdx];
                          const isCorrect = userSelected === q.answer;
                          
                          return (
                            <div 
                              key={qIdx}
                              className={`p-5 rounded-2xl border transition-all ${
                                darkMode ? 'bg-slate-950/40 border-slate-800' : 'bg-slate-50 border-slate-100'
                              }`}
                            >
                              <h4 className="font-bold text-sm sm:text-base text-slate-800 dark:text-slate-200 mb-3 flex items-start gap-2">
                                <span className="bg-emerald-100 dark:bg-emerald-950 text-emerald-700 dark:text-emerald-300 px-2.5 py-0.5 rounded-lg text-xs font-black">{qIdx + 1}</span>
                                <span className="leading-relaxed">{q.question}</span>
                              </h4>

                              <div className="grid grid-cols-1 sm:grid-cols-2 gap-2 mt-2">
                                {q.options.map((option, oIdx) => {
                                  const isOptionSelected = userSelected === option;
                                  let optionStyle = darkMode ? 'bg-slate-900 border-slate-800 text-slate-300' : 'bg-white border-slate-200 text-slate-700';
                                  
                                  if (isOptionSelected) {
                                    optionStyle = 'bg-indigo-500 text-white border-indigo-600';
                                  }
                                  
                                  if (testChecked) {
                                    if (option === q.answer) {
                                      optionStyle = 'bg-emerald-500 text-white border-emerald-600 shadow-lg shadow-emerald-500/20';
                                    } else if (isOptionSelected && !isCorrect) {
                                      optionStyle = 'bg-rose-500 text-white border-rose-600 shadow-lg shadow-rose-500/20';
                                    }
                                  }

                                  return (
                                    <button
                                      key={oIdx}
                                      onClick={() => handleTestOptionClick(qIdx, option)}
                                      disabled={testChecked}
                                      className={`p-3 text-left rounded-xl border font-semibold text-xs transition-all flex items-center justify-between ${optionStyle} ${!testChecked ? 'hover:scale-101' : ''}`}
                                    >
                                      <span>{option}</span>
                                      {isOptionSelected && !testChecked && <Check className="w-4 h-4 shrink-0 text-white" />}
                                      {testChecked && option === q.answer && <Check className="w-4 h-4 shrink-0 text-white animate-bounce" />}
                                      {testChecked && isOptionSelected && option !== q.answer && <span className="text-white text-[10px] font-bold">Қате</span>}
                                    </button>
                                  );
                                })}
                              </div>
                            </div>
                          );
                        })}
                      </div>

                      <div className="flex flex-col sm:flex-row justify-between items-center bg-slate-100 dark:bg-slate-950 p-5 rounded-2xl border gap-4 border-slate-200 dark:border-slate-850">
                        <div className="text-center sm:text-left">
                          <p className="text-sm font-bold text-slate-700 dark:text-slate-300">Интерактивті тест нәтижесі:</p>
                          <p className="text-xs text-slate-400 mt-1">Оқушылардың жауабын тексеру үшін сұрақтарды тақтада белгілеп, нәтижені басыңыз.</p>
                        </div>
                        
                        <div className="flex items-center gap-3">
                          {testChecked && (
                            <div className="px-4 py-2 bg-emerald-100 dark:bg-emerald-950 text-emerald-850 dark:text-emerald-300 font-extrabold text-sm rounded-xl border border-emerald-500/20">
                              Нәтиже: {calculateCorrectAnswers()} / {generatedData.test.length} дұрыс
                            </div>
                          )}
                          <button
                            onClick={() => setTestChecked(true)}
                            disabled={Object.keys(testAnswers).length === 0}
                            className={`py-2.5 px-6 rounded-xl font-bold text-xs shadow-md transition-all ${
                              Object.keys(testAnswers).length === 0 
                                ? 'bg-slate-300 text-slate-500 cursor-not-allowed dark:bg-slate-800' 
                                : 'bg-emerald-500 text-white hover:bg-emerald-600 hover:shadow-emerald-500/20'
                            }`}
                          >
                            Жауаптарды Тексеру
                          </button>
                        </div>
                      </div>
                    </div>
                  )}

                  {/* ТАВ 4: СӘЙКЕСТЕНДІРУ ОЙЫНЫ */}
                  {activeTab === 'matching' && (
                    <div className="space-y-6 animate-fadeIn">
                      <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center border-b pb-4 gap-2">
                        <div>
                          <h3 className="text-lg font-bold flex items-center gap-2 text-amber-600 dark:text-amber-400">
                            <Shuffle className="w-5 h-5" /> Сәйкестендіру Ойыны
                          </h3>
                          <p className="text-xs text-slate-400 mt-1">Оқушыларды белсендіру мен терминдерді есте сақтау үшін арналған интерактивті тапсырма.</p>
                        </div>
                        <button
                          onClick={() => prepareMatchingGame(generatedData.matching)}
                          className="text-xs text-indigo-500 hover:text-indigo-600 dark:hover:text-indigo-400 font-extrabold flex items-center gap-1"
                        >
                          <Shuffle className="w-3.5 h-3.5" /> Ойынды қайта бастау
                        </button>
                      </div>

                      {matchingFeedback && (
                        <div className={`p-3 rounded-xl border text-center text-xs font-bold animate-pulse ${
                          matchingFeedback.success 
                            ? 'bg-emerald-50 border-emerald-200 text-emerald-700 dark:bg-emerald-950 dark:border-emerald-800 dark:text-emerald-300' 
                            : 'bg-rose-50 border-rose-200 text-rose-700 dark:bg-rose-950 dark:border-rose-800 dark:text-rose-300'
                        }`}>
                          {matchingFeedback.message}
                        </div>
                      )}

                      <div className="grid grid-cols-1 md:grid-cols-2 gap-8">
                        <div className="space-y-3">
                          <h4 className="text-xs font-black uppercase text-slate-400 border-b pb-2 mb-2">Сол жақ баған (Ұғымдар)</h4>
                          {shuffledLeft.map((item) => {
                            const isMatched = matchedPairs.includes(item.id);
                            const isSelected = matchingSelections.left?.id === item.id;
                            
                            let btnStyle = darkMode ? 'bg-slate-950 border-slate-800 text-slate-300 hover:bg-slate-900' : 'bg-white border-slate-200 text-slate-700 hover:bg-slate-50';
                            if (isSelected) btnStyle = 'bg-amber-500 text-white border-amber-600 shadow-lg shadow-amber-500/20';
                            if (isMatched) btnStyle = 'bg-emerald-50 text-emerald-700 dark:bg-emerald-950/20 dark:text-emerald-400 border-emerald-200 opacity-60 cursor-not-allowed';

                            return (
                              <button
                                key={item.id}
                                disabled={isMatched}
                                onClick={() => handleSelectMatching('left', item)}
                                className={`w-full p-4 text-left rounded-2xl border font-bold text-xs sm:text-sm transition-all flex items-center justify-between ${btnStyle}`}
                              >
                                <span>{item.text}</span>
                                {isMatched && <Check className="w-4.5 h-4.5 text-emerald-500 shrink-0" />}
                              </button>
                            );
                          })}
                        </div>

                        <div className="space-y-3">
                          <h4 className="text-xs font-black uppercase text-slate-400 border-b pb-2 mb-2">Оң жақ баған (Сипаттамалар)</h4>
                          {shuffledRight.map((item) => {
                            const isMatched = matchedPairs.includes(item.id);
                            const isSelected = matchingSelections.right?.id === item.id;
                            
                            let btnStyle = darkMode ? 'bg-slate-950 border-slate-800 text-slate-300 hover:bg-slate-900' : 'bg-white border-slate-200 text-slate-700 hover:bg-slate-50';
                            if (isSelected) btnStyle = 'bg-amber-500 text-white border-amber-600 shadow-lg shadow-amber-500/20';
                            if (isMatched) btnStyle = 'bg-emerald-50 text-emerald-700 dark:bg-emerald-950/20 dark:text-emerald-400 border-emerald-200 opacity-60 cursor-not-allowed';

                            return (
                              <button
                                key={item.id}
                                disabled={isMatched}
                                onClick={() => handleSelectMatching('right', item)}
                                className={`w-full p-4 text-left rounded-2xl border font-medium text-xs sm:text-sm transition-all flex items-center justify-between ${btnStyle}`}
                              >
                                <span className="leading-relaxed">{item.text}</span>
                                {isMatched && <Check className="w-4.5 h-4.5 text-emerald-500 shrink-0" />}
                              </button>
                            );
                          })}
                        </div>
                      </div>

                      {matchedPairs.length === generatedData.matching.length && (
                        <div className="p-8 text-center bg-gradient-to-tr from-emerald-500/10 to-indigo-500/10 rounded-3xl border border-emerald-500/20 animate-fadeIn space-y-3">
                          <div className="bg-emerald-500 text-white w-12 h-12 rounded-full flex items-center justify-center text-xl mx-auto animate-bounce">🎉</div>
                          <h4 className="text-lg font-black text-emerald-600">Ойын сәтті аяқталды! Тамаша жұмыс!</h4>
                          <p className="text-xs text-slate-500 dark:text-slate-400">Барлық терминдер сәйкестендірілді. Оқушылар тақырыпты толық меңгерді деп санауға болады.</p>
                        </div>
                      )}
                    </div>
                  )}

                  {/* ТАВ 5: БАҒАЛАУ ӨЛШЕМДЕРІ ЖӘНЕ ДЕСКРИПТОРЛАР */}
                  {activeTab === 'assessment' && (
                    <div className="space-y-6 animate-fadeIn">
                      <div className="border-b pb-4">
                        <h3 className="text-lg font-bold flex items-center gap-2 text-indigo-600 dark:text-indigo-400">
                          <Award className="w-5 h-5" /> Сабақтың бағалау өлшемдері мен критерийлері
                        </h3>
                        <p className="text-xs text-slate-400 mt-1">Оқушылардың білімін бағалауға арналған нақты дескрипторлардың кестесі</p>
                      </div>

                      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                        {(generatedData.assessmentRubrics || [
                          {
                            criteria: "Мазмұнды меңгеру деңгейі",
                            descriptors: [
                              "Төмен: Тақырып терминдерін қолдана алмайды",
                              "Орта: Тақырыпты сипаттайды, бірақ талдауда қателер жібереді",
                              "Жоғары: Тақырыпты еркін талдап, қорытындылар жасай алады"
                            ]
                          }
                        ]).map((rubric, rIdx) => (
                          <div key={rIdx} className="p-5 rounded-3xl border bg-slate-50 dark:bg-slate-950 border-slate-200 dark:border-slate-850 shadow-sm space-y-4">
                            <h4 className="font-extrabold text-sm text-indigo-600 dark:text-indigo-400 flex items-center gap-2">
                              <span className="bg-indigo-100 dark:bg-indigo-950 w-6 h-6 rounded-lg flex items-center justify-center text-xs text-indigo-700 dark:text-indigo-300 font-bold">{rIdx+1}</span>
                              <span>{rubric.criteria}</span>
                            </h4>
                            <div className="space-y-2">
                              {rubric.descriptors.map((desc, dIdx) => (
                                <div key={dIdx} className="p-3 rounded-xl bg-white dark:bg-slate-900 border text-xs leading-relaxed font-semibold flex gap-2">
                                  <CornerDownRight className="w-4 h-4 text-emerald-500 shrink-0" />
                                  <span className="text-slate-600 dark:text-slate-300">{desc}</span>
                                </div>
                              ))}
                            </div>
                          </div>
                        ))}
                      </div>

                      {/* Кері байланыс exit ticket */}
                      {generatedData.exitTicket && (
                        <div className="p-6 rounded-3xl border bg-gradient-to-r from-indigo-500/5 to-purple-500/5 border-indigo-500/10 space-y-4">
                          <div className="flex items-center gap-2">
                            <BookOpenCheck className="w-5 h-5 text-indigo-500" />
                            <h4 className="text-sm font-black uppercase text-indigo-500 tracking-wider">Кері байланыс картасы: {generatedData.exitTicket.title}</h4>
                          </div>
                          <p className="text-xs text-slate-500 dark:text-slate-400">Сабақ соңында оқушылардан мына сұрақтар бойынша жауап алу ұсынылады:</p>
                          <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                            {generatedData.exitTicket.bullets.map((bullet, idx) => (
                              <div key={idx} className="p-4 rounded-2xl bg-white dark:bg-slate-900 border text-xs font-bold text-slate-700 dark:text-slate-300 flex flex-col justify-between shadow-sm">
                                <span className="text-indigo-500 font-black text-sm mb-2">{idx+1}➔</span>
                                <p className="leading-relaxed">{bullet}</p>
                              </div>
                            ))}
                          </div>
                        </div>
                      )}

                      {/* Инклюзивті қолдау */}
                      {generatedData.senAdaptation && (
                        <div className="p-5 rounded-2xl bg-teal-500/5 border border-teal-500/20 space-y-2 relative overflow-hidden">
                          <div className="absolute top-0 left-0 w-1.5 h-full bg-teal-500"></div>
                          <div className="flex items-center gap-2">
                            <HeartHandshake className="w-5 h-5 text-teal-600 dark:text-teal-400" />
                            <h4 className="text-xs font-black text-teal-600 dark:text-teal-400 uppercase tracking-widest">Инклюзивті оқытуды қолдау (SEN Adaptation)</h4>
                          </div>
                          <p className="text-xs text-slate-600 dark:text-slate-300 font-semibold leading-relaxed pl-3 italic">
                            "{generatedData.senAdaptation}"
                          </p>
                        </div>
                      )}
                    </div>
                  )}

                  {/* ТАВ 6: КӨРНЕКІЛІКТЕР */}
                  {activeTab === 'visuals' && (
                    <div className="space-y-6 animate-fadeIn">
                      <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center border-b pb-4 gap-2">
                        <div>
                          <h3 className="text-lg font-bold flex items-center gap-2 text-purple-600 dark:text-purple-400">
                            <ImageIcon className="w-5 h-5" /> Сабаққа арналған Көрнекіліктер
                          </h3>
                          <p className="text-xs text-slate-400 mt-1">Оқу мақсатына сәйкес арнайы плакаттар мен сипаттамалы суреттерді Imagen 4.0 арқылы тікелей жасау</p>
                        </div>
                      </div>

                      <div className="p-5 rounded-2xl bg-indigo-500/5 border border-indigo-500/10 text-xs sm:text-sm text-slate-600 dark:text-slate-300 font-medium leading-relaxed">
                        <span className="font-bold text-indigo-500 block mb-1">Мұғалімге көрнекілікті қолдану бойынша әдістемелік кеңес:</span>
                        {generatedData.visualDescriptionKazakh}
                      </div>

                      <div className="border border-dashed rounded-3xl p-6 sm:p-10 flex flex-col items-center text-center justify-center space-y-6">
                        {!isGeneratingImage && !generatedImageUrl && (
                          <>
                            <div className="bg-purple-100 dark:bg-purple-950/40 p-5 rounded-full text-purple-600">
                              <ImageIcon className="w-12 h-12" />
                            </div>
                            <div className="space-y-1">
                              <h4 className="font-bold text-base">Көрнекі плакат генерациясы</h4>
                              <p className="text-xs text-slate-400 max-w-md">"Imagen 4.0" озық нейрожелісін пайдалана отырып, осы сабаққа арналған ерекше көрнекілік плакатын жасаңыз.</p>
                            </div>
                            
                            <div className="bg-slate-50 dark:bg-slate-900 p-3 rounded-xl border border-dashed text-left w-full max-w-md">
                              <span className="text-[9px] font-bold text-slate-400 block mb-1 uppercase tracking-wider">Сурет генерациялау промпты (AI үшін):</span>
                              <p className="text-[11px] font-medium text-slate-600 dark:text-slate-400 italic font-mono">{generatedData.visualPrompt}</p>
                            </div>

                            <button
                              onClick={generateVisualImage}
                              className="py-3 px-6 rounded-2xl bg-purple-600 hover:bg-purple-700 text-white font-bold text-xs shadow-lg shadow-purple-500/15 flex items-center gap-2 transition-all hover:-translate-y-0.5"
                            >
                              <Sparkles className="w-4 h-4" />
                              <span>Плакатты Суретке айналдыру</span>
                            </button>
                          </>
                        )}

                        {isGeneratingImage && (
                          <div className="space-y-4 py-8">
                            <div className="w-16 h-16 border-4 border-purple-100 border-t-purple-600 rounded-full animate-spin mx-auto"></div>
                            <p className="text-sm font-bold text-purple-600">Imagen 4.0 көрнекі суретті жасап жатыр...</p>
                          </div>
                        )}

                        {imageError && (
                          <div className="p-4 rounded-xl bg-rose-50 border border-rose-100 text-rose-600 flex items-start gap-2 max-w-md text-left">
                            <AlertCircle className="w-5 h-5 shrink-0 mt-0.5" />
                            <div>
                              <p className="text-xs font-bold">Сурет генерациясы орындалмады</p>
                              <p className="text-[11px] leading-relaxed">{imageError}</p>
                            </div>
                          </div>
                        )}

                        {generatedImageUrl && (
                          <div className="space-y-4 w-full">
                            <div className="relative rounded-2xl overflow-hidden border shadow-xl bg-black max-w-lg mx-auto group">
                              <img src={generatedImageUrl} alt="Сабақ көрнекілігі" className="w-full h-auto object-contain max-h-[450px]" />
                              <div className="absolute inset-0 bg-black/40 opacity-0 group-hover:opacity-100 transition-opacity flex items-center justify-center">
                                <a 
                                  href={generatedImageUrl} 
                                  download={`${generatedData.lessonPlan.title}-көрнекілік.png`}
                                  className="p-3 bg-white text-slate-800 rounded-full font-bold hover:scale-110 transition-transform shadow-lg flex items-center gap-1 text-xs"
                                >
                                  <Download className="w-4 h-4" />
                                  <span>Жүктеп алу</span>
                                </a>
                              </div>
                            </div>
                            <button onClick={generateVisualImage} className="py-2 px-4 rounded-xl border border-purple-500/30 text-purple-600 text-xs font-bold transition-all">Басқаша сурет жасау</button>
                          </div>
                        )}
                      </div>
                    </div>
                  )}

                  {/* ТАВ 7: ТАПСЫРМАЛАР (Жеке, Жұптық, Топтық, Үй) */}
                  {activeTab === 'tasks' && (
                    <div className="space-y-6 animate-fadeIn">
                      <div className="flex justify-between items-center border-b pb-4">
                        <div className="space-y-1">
                          <h3 className="text-lg font-bold flex items-center gap-2 text-sky-600 dark:text-sky-400">
                            <PenTool className="w-5 h-5" /> Сабақ барысындағы тапсырмалар
                          </h3>
                          <p className="text-xs text-slate-400">Оқушыларды сабаққа тартуға арналған дифференциацияланған жұмыс түрлері.</p>
                        </div>
                        <button 
                          onClick={() => copyToClipboard(`Жеке жұмыс:\n${generatedData.practicalTasks?.individual}\n\nЖұптық жұмыс:\n${generatedData.practicalTasks?.pair}\n\nТоптық жұмыс:\n${generatedData.practicalTasks?.group}`)}
                          className="text-xs text-slate-400 hover:text-slate-600 dark:hover:text-slate-200 flex items-center gap-1 hidden sm:flex"
                        >
                          <Clipboard className="w-3.5 h-3.5" /> Барлығын көшіру
                        </button>
                      </div>

                      <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
                        {/* Жеке жұмыс */}
                        <div className="p-6 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 shadow-sm relative overflow-hidden group hover:border-blue-400 dark:hover:border-blue-500/50 transition-colors">
                          <div className="absolute top-0 left-0 w-1.5 h-full bg-blue-500"></div>
                          <h4 className="flex items-center gap-2 font-bold text-blue-600 dark:text-blue-400 mb-3 uppercase text-xs tracking-wider">
                            <PenTool className="w-4 h-4" /> Жеке жұмыс
                          </h4>
                          <p className="text-sm text-slate-700 dark:text-slate-300 whitespace-pre-line leading-relaxed font-medium">
                            {generatedData.practicalTasks?.individual || "Тапсырма генерацияланбаған немесе ескі нұсқада сақталған."}
                          </p>
                        </div>

                        {/* Жұптық жұмыс */}
                        <div className="p-6 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 shadow-sm relative overflow-hidden group hover:border-amber-400 dark:hover:border-amber-500/50 transition-colors">
                          <div className="absolute top-0 left-0 w-1.5 h-full bg-amber-500"></div>
                          <h4 className="flex items-center gap-2 font-bold text-amber-600 dark:text-amber-400 mb-3 uppercase text-xs tracking-wider">
                            <HeartHandshake className="w-4 h-4" /> Жұптық жұмыс
                          </h4>
                          <p className="text-sm text-slate-700 dark:text-slate-300 whitespace-pre-line leading-relaxed font-medium">
                            {generatedData.practicalTasks?.pair || "Тапсырма генерацияланбаған немесе ескі нұсқада сақталған."}
                          </p>
                        </div>

                        {/* Топтық жұмыс */}
                        <div className="p-6 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 shadow-sm relative overflow-hidden group hover:border-purple-400 dark:hover:border-purple-500/50 transition-colors">
                          <div className="absolute top-0 left-0 w-1.5 h-full bg-purple-500"></div>
                          <h4 className="flex items-center gap-2 font-bold text-purple-600 dark:text-purple-400 mb-3 uppercase text-xs tracking-wider">
                            <Users className="w-4 h-4" /> Топтық жұмыс
                          </h4>
                          <p className="text-sm text-slate-700 dark:text-slate-300 whitespace-pre-line leading-relaxed font-medium">
                            {generatedData.practicalTasks?.group || "Тапсырма генерацияланбаған немесе ескі нұсқада сақталған."}
                          </p>
                        </div>
                      </div>
                    </div>
                  )}

                  {/* ЖАҢА ТАВ: ҮЙ ТАПСЫРМАСЫ */}
                  {activeTab === 'homework' && (
                    <div className="space-y-6 animate-fadeIn">
                      <div className="flex justify-between items-center border-b pb-4">
                        <div className="space-y-1">
                          <h3 className="text-lg font-bold flex items-center gap-2 text-teal-600 dark:text-teal-400">
                            <Home className="w-5 h-5" /> Үй тапсырмасы
                          </h3>
                          <p className="text-xs text-slate-400">Оқушыларға сабақтан тыс уақытта орындауға берілетін тапсырма мен нұсқаулық.</p>
                        </div>
                        <button 
                          onClick={() => copyToClipboard(`Үй тапсырмасы:\n${generatedData.homework}`)}
                          className="text-xs text-slate-400 hover:text-slate-600 dark:hover:text-slate-200 flex items-center gap-1 hidden sm:flex"
                        >
                          <Clipboard className="w-3.5 h-3.5" /> Көшіру
                        </button>
                      </div>

                      <div className="p-8 rounded-3xl bg-teal-50 dark:bg-slate-950 border border-teal-200 dark:border-teal-900/50 shadow-sm relative overflow-hidden group hover:border-teal-400 dark:hover:border-teal-500/50 transition-colors">
                        <div className="absolute top-0 left-0 w-2 h-full bg-teal-500"></div>
                        <p className="text-base text-slate-700 dark:text-slate-300 whitespace-pre-line leading-relaxed font-medium pl-2">
                          {generatedData.homework || "Тапсырма табылмады."}
                        </p>
                      </div>
                    </div>
                  )}

                  {/* ТАВ 8: РЕСУРСТАР МЕН СІЛТЕМЕЛЕР (Жетілдірілген мұғалім өңдегішімен) */}
                  {activeTab === 'resources' && (
                    <div className="space-y-6 animate-fadeIn">
                      <div className="flex justify-between items-center border-b pb-4">
                        <div className="space-y-1">
                          <h3 className="text-lg font-bold flex items-center gap-2 text-indigo-600 dark:text-indigo-400">
                            <Link2 className="w-5 h-5" /> Тақырыптық Ресурстар мен Смарт Сілтемелер
                          </h3>
                          <p className="text-xs text-slate-400">Сілтемелерді өңдеуге, өшіруге немесе өзіңіздің жеке сілтемелеріңізді қолмен қосуға болады.</p>
                        </div>
                      </div>

                      {/* Өңдеу немесе Қосу инлайн-формасы */}
                      {(editingResource || addingToCategory) && (
                        <div className="p-5 rounded-3xl border-2 border-indigo-500 bg-indigo-50/50 dark:bg-slate-900/50 space-y-4 animate-slideDown">
                          <h4 className="font-extrabold text-sm text-indigo-600 dark:text-indigo-400 flex items-center gap-2">
                            <Edit2 className="w-4 h-4" />
                            <span>{addingToCategory ? 'Жаңа ресурс қосу' : 'Ресурсты өңдеу'}</span>
                          </h4>

                          <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div>
                              <label className="block text-[10px] font-bold uppercase text-slate-400 mb-1">Ресурс тақырыбы</label>
                              <input 
                                type="text"
                                value={editForm.title}
                                onChange={(e) => setEditForm({ ...editForm, title: e.target.value })}
                                className="w-full p-2.5 text-xs rounded-xl border dark:bg-slate-800 dark:border-slate-750"
                                placeholder="Мысалы: Физикалық тәжірибе видеосы"
                              />
                            </div>
                            <div>
                              <label className="block text-[10px] font-bold uppercase text-slate-400 mb-1">Сілтеме/URL (searchUrl немесе url)</label>
                              <input 
                                type="text"
                                value={editForm.url}
                                onChange={(e) => setEditForm({ ...editForm, url: e.target.value })}
                                className="w-full p-2.5 text-xs rounded-xl border dark:bg-slate-800 dark:border-slate-750"
                                placeholder="https://example.com"
                              />
                            </div>

                            {/* Қосымша баптаулар */}
                            {((editingResource && editingResource.category === 'games') || addingToCategory === 'games') && (
                              <>
                                <div>
                                  <label className="block text-[10px] font-bold uppercase text-slate-400 mb-1">Ойын платформасы (Wordwall, Kahoot, т.б.)</label>
                                  <input 
                                    type="text"
                                    value={editForm.platformOrSource}
                                    onChange={(e) => setEditForm({ ...editForm, platformOrSource: e.target.value })}
                                    className="w-full p-2.5 text-xs rounded-xl border dark:bg-slate-800 dark:border-slate-750"
                                    placeholder="Wordwall"
                                  />
                                </div>
                                <div>
                                  <label className="block text-[10px] font-bold uppercase text-slate-400 mb-1">Қысқаша сипаттамасы</label>
                                  <input 
                                    type="text"
                                    value={editForm.desc}
                                    onChange={(e) => setEditForm({ ...editForm, desc: e.target.value })}
                                    className="w-full p-2.5 text-xs rounded-xl border dark:bg-slate-800 dark:border-slate-750"
                                    placeholder="Оқушыларға арналған сәйкестендіру тапсырмасы"
                                  />
                                </div>
                              </>
                            )}

                            {((editingResource && editingResource.category === 'websites') || addingToCategory === 'websites') && (
                              <div>
                                <label className="block text-[10px] font-bold uppercase text-slate-400 mb-1">Дереккөз атауы (BilimLand, Wikipedia, т.б.)</label>
                                <input 
                                  type="text"
                                  value={editForm.platformOrSource}
                                  onChange={(e) => setEditForm({ ...editForm, platformOrSource: e.target.value })}
                                  className="w-full p-2.5 text-xs rounded-xl border dark:bg-slate-800 dark:border-slate-750"
                                  placeholder="Bilimland.kz"
                                />
                              </div>
                            )}
                          </div>

                          <div className="flex gap-2 justify-end pt-2">
                            <button
                              onClick={() => {
                                setEditingResource(null);
                                setAddingToCategory(null);
                                setEditForm({ title: '', url: '', desc: '', platformOrSource: '' });
                              }}
                              className="py-1.5 px-4 rounded-xl border text-xs font-bold hover:bg-slate-100 dark:hover:bg-slate-800"
                            >
                              Болдырмау
                            </button>
                            <button
                              onClick={addingToCategory ? () => handleAddCustomResource(addingToCategory) : handleSaveResourceEdit}
                              className="py-1.5 px-5 bg-indigo-500 hover:bg-indigo-600 text-white font-bold text-xs rounded-xl"
                            >
                              {addingToCategory ? 'Қосу' : 'Сақтау'}
                            </button>
                          </div>
                        </div>
                      )}

                      <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
                        
                        {/* 1. YouTube Колоннасы */}
                        <div className={`p-5 rounded-3xl border flex flex-col justify-between ${darkMode ? 'bg-slate-900/40 border-slate-800' : 'bg-slate-50 border-slate-200'}`}>
                          <div className="space-y-4">
                            <div className="flex items-center justify-between text-red-500 border-b pb-2 border-slate-100 dark:border-slate-800">
                              <div className="flex items-center gap-2">
                                <Youtube className="w-5 h-5" />
                                <h4 className="font-extrabold text-sm uppercase">YouTube Бейнелері</h4>
                              </div>
                              <button
                                onClick={() => {
                                  setAddingToCategory('youtube');
                                  setEditingResource(null);
                                  setEditForm({ title: '', url: '', desc: '', platformOrSource: '' });
                                }}
                                className="p-1 hover:bg-red-50 dark:hover:bg-red-950/20 text-red-500 rounded-lg"
                                title="Жаңа YouTube сілтемесін қосу"
                              >
                                <Plus className="w-4 h-4" />
                              </button>
                            </div>
                            
                            <div className="space-y-3">
                              {(generatedData.externalResources?.youtube || []).map((item, idx) => (
                                <div key={idx} className="p-3 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-850 shadow-sm space-y-2 hover:shadow-md transition-all group relative">
                                  <span className="text-xs font-bold text-slate-700 dark:text-slate-200 block pr-12 leading-tight">
                                    {item.title}
                                  </span>
                                  <a 
                                    href={item.searchUrl || item.url} 
                                    target="_blank" 
                                    rel="noopener noreferrer"
                                    className="inline-flex items-center gap-1 text-[11px] font-bold text-red-500 hover:text-red-600 hover:underline"
                                  >
                                    <span>Тікелей сілтемені ашу</span>
                                    <ExternalLink className="w-3 h-3" />
                                  </a>

                                  <div className="absolute top-2 right-2 flex gap-1 opacity-0 group-hover:opacity-100 transition-opacity">
                                    <button 
                                      onClick={() => handleStartEditResource('youtube', idx, item)}
                                      className="p-1 bg-slate-100 hover:bg-indigo-100 text-slate-500 hover:text-indigo-600 rounded-lg dark:bg-slate-800"
                                      title="Өңдеу"
                                    >
                                      <Edit2 className="w-3 h-3" />
                                    </button>
                                    <button 
                                      onClick={() => handleDeleteResource('youtube', idx)}
                                      className="p-1 bg-slate-100 hover:bg-rose-100 text-slate-500 hover:text-rose-600 rounded-lg dark:bg-slate-800"
                                      title="Өшіру"
                                    >
                                      <Trash2 className="w-3 h-3" />
                                    </button>
                                  </div>
                                </div>
                              ))}
                            </div>
                          </div>

                          <div className="pt-4 border-t border-dashed mt-4 space-y-2 border-slate-200 dark:border-slate-800">
                            <label className="text-[10px] font-bold text-slate-400 uppercase tracking-wider block">Іздеу сұранысын түзету:</label>
                            <div className="flex gap-1.5">
                              <input 
                                type="text"
                                value={customYoutubeQuery}
                                onChange={(e) => setCustomYoutubeQuery(e.target.value)}
                                className={`flex-1 px-2.5 py-1.5 text-xs rounded-xl border focus:outline-none focus:ring-1 focus:ring-red-500 ${
                                  darkMode ? 'bg-slate-950 border-slate-800 text-slate-100' : 'bg-white border-slate-200 text-slate-800'
                                }`}
                              />
                              <a
                                href={`https://www.youtube.com/results?search_query=${encodeURIComponent(customYoutubeQuery)}`}
                                target="_blank"
                                rel="noopener noreferrer"
                                className="p-2 bg-red-500 hover:bg-red-600 text-white rounded-xl shadow-sm transition-all"
                              >
                                <Search className="w-3.5 h-3.5" />
                              </a>
                            </div>
                          </div>
                        </div>

                        {/* 2. Ойындар Колоннасы */}
                        <div className={`p-5 rounded-3xl border flex flex-col justify-between ${darkMode ? 'bg-slate-900/40 border-slate-800' : 'bg-slate-50 border-slate-200'}`}>
                          <div className="space-y-4">
                            <div className="flex items-center justify-between text-emerald-500 border-b pb-2 border-slate-100 dark:border-slate-800">
                              <div className="flex items-center gap-2">
                                <Shuffle className="w-5 h-5" />
                                <h4 className="font-extrabold text-sm uppercase">Ойындар мен Квиздер</h4>
                              </div>
                              <button
                                onClick={() => {
                                  setAddingToCategory('games');
                                  setEditingResource(null);
                                  setEditForm({ title: '', url: '', desc: '', platformOrSource: '' });
                                }}
                                className="p-1 hover:bg-emerald-50 dark:hover:bg-emerald-950/20 text-emerald-500 rounded-lg"
                                title="Жаңа ойын сілтемесін қосу"
                              >
                                <Plus className="w-4 h-4" />
                              </button>
                            </div>

                            <div className="space-y-3">
                              {(generatedData.externalResources?.games || []).map((item, idx) => (
                                <div key={idx} className="p-3 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-850 shadow-sm space-y-1.5 hover:shadow-md transition-all group relative">
                                  <div className="flex justify-between items-center pr-10">
                                    <span className="text-xs font-bold text-slate-700 dark:text-slate-200 block truncate max-w-[130px]">
                                      {item.title}
                                    </span>
                                    <span className="text-[9px] font-extrabold px-1.5 py-0.5 bg-emerald-100 text-emerald-800 rounded-full shrink-0">
                                      {item.platform}
                                    </span>
                                  </div>
                                  <p className="text-[10px] text-slate-400 leading-tight">
                                    {item.description}
                                  </p>
                                  <a 
                                    href={item.searchUrl || item.url} 
                                    target="_blank" 
                                    rel="noopener noreferrer"
                                    className="inline-flex items-center gap-1 text-[11px] font-bold text-emerald-500 hover:text-emerald-600 hover:underline"
                                  >
                                    <span>Тікелей сілтемені ашу</span>
                                    <ExternalLink className="w-3 h-3" />
                                  </a>

                                  <div className="absolute top-2 right-2 flex gap-1 opacity-0 group-hover:opacity-100 transition-opacity">
                                    <button 
                                      onClick={() => handleStartEditResource('games', idx, item)}
                                      className="p-1 bg-slate-100 hover:bg-indigo-100 text-slate-500 hover:text-indigo-600 rounded-lg dark:bg-slate-800"
                                      title="Өңдеу"
                                    >
                                      <Edit2 className="w-3 h-3" />
                                    </button>
                                    <button 
                                      onClick={() => handleDeleteResource('games', idx)}
                                      className="p-1 bg-slate-100 hover:bg-rose-100 text-slate-500 hover:text-rose-600 rounded-lg dark:bg-slate-800"
                                      title="Өшіру"
                                    >
                                      <Trash2 className="w-3 h-3" />
                                    </button>
                                  </div>
                                </div>
                              ))}
                            </div>
                          </div>

                          <div className="pt-4 border-t border-dashed mt-4 space-y-2 border-slate-200 dark:border-slate-800">
                            <label className="text-[10px] font-bold text-slate-400 uppercase tracking-wider block">Іздеу сұранысын түзету:</label>
                            <div className="flex gap-1.5">
                              <input 
                                type="text"
                                value={customGamesQuery}
                                onChange={(e) => setCustomGamesQuery(e.target.value)}
                                className={`flex-1 px-2.5 py-1.5 text-xs rounded-xl border focus:outline-none focus:ring-1 focus:ring-emerald-500 ${
                                  darkMode ? 'bg-slate-950 border-slate-800 text-slate-100' : 'bg-white border-slate-200 text-slate-800'
                                }`}
                              />
                              <a
                                href={`https://wordwall.net/ru/community?query=${encodeURIComponent(customGamesQuery)}`}
                                target="_blank"
                                rel="noopener noreferrer"
                                className="p-2 bg-emerald-500 hover:bg-emerald-600 text-white rounded-xl shadow-sm transition-all"
                              >
                                <Search className="w-3.5 h-3.5" />
                              </a>
                            </div>
                          </div>
                        </div>

                        {/* 3. Сайттар Колоннасы */}
                        <div className={`p-5 rounded-3xl border flex flex-col justify-between ${darkMode ? 'bg-slate-900/40 border-slate-800' : 'bg-slate-50 border-slate-200'}`}>
                          <div className="space-y-4">
                            <div className="flex items-center justify-between text-indigo-500 border-b pb-2 border-slate-100 dark:border-slate-800">
                              <div className="flex items-center gap-2">
                                <Globe className="w-5 h-5" />
                                <h4 className="font-extrabold text-sm uppercase">Сайттар мен Мақалалар</h4>
                              </div>
                              <button
                                onClick={() => {
                                  setAddingToCategory('websites');
                                  setEditingResource(null);
                                  setEditForm({ title: '', url: '', desc: '', platformOrSource: '' });
                                }}
                                className="p-1 hover:bg-indigo-50 dark:hover:bg-indigo-950/20 text-indigo-500 rounded-lg"
                                title="Жаңа веб-сайт қосу"
                              >
                                <Plus className="w-4 h-4" />
                              </button>
                            </div>

                            <div className="space-y-3">
                              {(generatedData.externalResources?.websites || []).map((item, idx) => (
                                <div key={idx} className="p-3 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-850 shadow-sm space-y-2 hover:shadow-md transition-all group relative">
                                  <div className="flex justify-between items-center pr-10">
                                    <span className="text-xs font-bold text-slate-700 dark:text-slate-200 block truncate max-w-[130px]">
                                      {item.title}
                                    </span>
                                    <span className="text-[9px] font-extrabold px-1.5 py-0.5 bg-indigo-100 text-indigo-800 rounded-full shrink-0">
                                      {item.sourceName}
                                    </span>
                                  </div>
                                  <a 
                                    href={item.url || item.searchUrl} 
                                    target="_blank" 
                                    rel="noopener noreferrer"
                                    className="inline-flex items-center gap-1 text-[11px] font-bold text-indigo-500 hover:text-indigo-600 hover:underline"
                                  >
                                    <span>Ресурсты ашу</span>
                                    <ExternalLink className="w-3 h-3" />
                                  </a>

                                  <div className="absolute top-2 right-2 flex gap-1 opacity-0 group-hover:opacity-100 transition-opacity">
                                    <button 
                                      onClick={() => handleStartEditResource('websites', idx, item)}
                                      className="p-1 bg-slate-100 hover:bg-indigo-100 text-slate-500 hover:text-indigo-600 rounded-lg dark:bg-slate-800"
                                      title="Өңдеу"
                                    >
                                      <Edit2 className="w-3 h-3" />
                                    </button>
                                    <button 
                                      onClick={() => handleDeleteResource('websites', idx)}
                                      className="p-1 bg-slate-100 hover:bg-rose-100 text-slate-500 hover:text-rose-600 rounded-lg dark:bg-slate-800"
                                      title="Өшіру"
                                    >
                                      <Trash2 className="w-3 h-3" />
                                    </button>
                                  </div>
                                </div>
                              ))}
                            </div>
                          </div>

                          <div className="pt-4 border-t border-dashed mt-4 space-y-2 border-slate-200 dark:border-slate-800">
                            <label className="text-[10px] font-bold text-slate-400 uppercase tracking-wider block">Іздеу сұранысын түзету:</label>
                            <div className="flex gap-1.5">
                              <input 
                                type="text"
                                value={customWebQuery}
                                onChange={(e) => setCustomWebQuery(e.target.value)}
                                className={`flex-1 px-2.5 py-1.5 text-xs rounded-xl border focus:outline-none focus:ring-1 focus:ring-indigo-500 ${
                                  darkMode ? 'bg-slate-950 border-slate-800 text-slate-100' : 'bg-white border-slate-200 text-slate-800'
                                }`}
                              />
                              <a
                                href={`https://www.google.com/search?q=${encodeURIComponent(customWebQuery)}`}
                                target="_blank"
                                rel="noopener noreferrer"
                                className="p-2 bg-indigo-500 hover:bg-indigo-600 text-white rounded-xl shadow-sm transition-all"
                              >
                                <Search className="w-3.5 h-3.5" />
                              </a>
                            </div>
                          </div>
                        </div>

                      </div>
                    </div>
                  )}

                  {/* ТАВ 9: СЫНЫП ҚҰРАЛДАРЫ */}
                  {activeTab === 'tools' && (
                    <div className="space-y-8 animate-fadeIn">
                      <div className="border-b pb-4">
                        <h3 className="text-lg font-bold flex items-center gap-2 text-indigo-600 dark:text-indigo-400">
                          <Users className="w-5 h-5" /> Интерактивті сынып құралдары
                        </h3>
                        <p className="text-xs text-slate-400 mt-1">Интерактивті тақтада оқушылар белсенділігі мен сақталатын таймер уақытын бақылауға арналған құрылғылар.</p>
                      </div>

                      <div className="grid grid-cols-1 md:grid-cols-2 gap-8">
                        
                        {/* 1. Кездейсоқ Оқушы таңдау құралы */}
                        <div className="p-6 rounded-3xl border bg-slate-50 dark:bg-slate-950/40 space-y-4 border-slate-200 dark:border-slate-850 shadow-sm">
                          <div className="flex items-center justify-between text-indigo-500 border-b pb-2 border-slate-150 dark:border-slate-850">
                            <div className="flex items-center gap-2">
                              <Users className="w-5 h-5" />
                              <h4 className="font-extrabold text-sm sm:text-base">Оқушыларды басқару</h4>
                            </div>
                            <span className="text-xs font-bold bg-indigo-100 dark:bg-indigo-950 text-indigo-700 dark:text-indigo-300 px-2.5 py-0.5 rounded-full">
                              Сыныпта: {students.length} оқушы
                            </span>
                          </div>

                          <div className="space-y-3">
                            <form onSubmit={handleAddStudent} className="flex gap-2">
                              <input
                                type="text"
                                value={newStudent}
                                onChange={(e) => setNewStudent(e.target.value)}
                                placeholder="Жаңа оқушының аты-жөні..."
                                className={`flex-1 px-3 py-2 rounded-xl border text-xs focus:outline-none focus:ring-2 focus:ring-indigo-500 ${
                                  darkMode ? 'bg-slate-900 border-slate-800 text-slate-100' : 'bg-white border-slate-200'
                                }`}
                              />
                              <button
                                type="submit"
                                className="px-4 py-2 bg-indigo-500 hover:bg-indigo-600 text-white font-bold text-xs rounded-xl flex items-center gap-1 transition-all shrink-0 shadow-md shadow-indigo-500/10"
                              >
                                <Plus className="w-3.5 h-3.5" />
                                <span>Қосу</span>
                              </button>
                            </form>

                            <div className="space-y-1">
                              <span className="text-[10px] font-bold text-slate-400 uppercase tracking-wider block">Сынып тізімі (Өшіру үшін ✕ басыңыз):</span>
                              <div className="flex flex-wrap gap-1.5 max-h-[120px] overflow-y-auto p-2 border rounded-xl bg-white dark:bg-slate-900">
                                {students.length === 0 ? (
                                  <span className="text-xs text-slate-400 italic">Тізім бос. Оқушыларды қосыңыз...</span>
                                ) : (
                                  students.map((st, sIdx) => (
                                    <span
                                      key={sIdx}
                                      className={`inline-flex items-center gap-1 px-2.5 py-1 rounded-lg text-xs font-semibold transition-all ${
                                        darkMode 
                                          ? 'bg-slate-805 text-slate-300 border border-slate-700' 
                                          : 'bg-slate-100 text-slate-700 border border-slate-200'
                                      }`}
                                    >
                                      <span>{st}</span>
                                      <button
                                        type="button"
                                        onClick={() => handleRemoveStudent(sIdx)}
                                        className="text-slate-400 hover:text-rose-500 font-bold ml-1 text-[10px]"
                                        title="Өшіру"
                                      >
                                        ✕
                                      </button>
                                    </span>
                                  ))
                                )}
                              </div>
                            </div>

                            <div className="flex gap-2 text-xs">
                              <button
                                type="button"
                                onClick={() => {
                                  setStudents([]);
                                  localStorage.setItem('ustaz_students', JSON.stringify([]));
                                  showNotification('Сынып тізімі тазартылды.', 'info');
                                }}
                                className="flex-1 py-1.5 border border-dashed border-rose-500/30 text-rose-500 hover:bg-rose-500/5 rounded-xl font-bold text-[10px] transition-all"
                              >
                                Тізімді тазарту
                              </button>
                              <button
                                type="button"
                                onClick={() => {
                                  const demoList = ["Әлібек", "Аружан", "Бауыржан", "Диана", "Ерасыл", "Жансая", "Мадияр", "Нұрасыл", "Сабина", "Темірлан"];
                                  setStudents(demoList);
                                  localStorage.setItem('ustaz_students', JSON.stringify(demoList));
                                  showNotification('Тізім бастапқы қалпына келтірілді.', 'info');
                                }}
                                className={`flex-1 py-1.5 border rounded-xl font-bold text-[10px] transition-all ${
                                  darkMode ? 'border-slate-800 text-slate-300 hover:bg-slate-800' : 'border-slate-200 text-slate-600 hover:bg-slate-100'
                                }`}
                              >
                                Бастапқы тізім
                              </button>
                            </div>

                            <div className="grid grid-cols-1 sm:grid-cols-2 gap-2 pt-2">
                              <button
                                type="button"
                                onClick={startStudentSpin}
                                disabled={isSpinning}
                                className={`py-2.5 rounded-xl font-bold text-xs text-white shadow-md transition-all flex items-center justify-center gap-1.5 ${
                                  isSpinning ? 'bg-slate-500 cursor-not-allowed' : 'bg-indigo-500 hover:bg-indigo-600'
                                }`}
                              >
                                <Shuffle className="w-4 h-4" />
                                <span>Кездейсоқ таңдау</span>
                              </button>

                              <button
                                type="button"
                                onClick={handleMakePairs}
                                className="py-2.5 rounded-xl font-bold text-xs text-white shadow-md transition-all flex items-center justify-center gap-1.5 bg-emerald-500 hover:bg-emerald-600"
                              >
                                <Users className="w-4 h-4" />
                                <span>Жұпқа бөлу</span>
                              </button>
                            </div>

                            <div className="flex gap-2 items-center bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 p-2 rounded-xl shadow-sm">
                              <span className="text-[11px] font-bold text-slate-500 pl-2">Топ саны:</span>
                              <input 
                                type="number"
                                min="2"
                                max="10"
                                value={numberOfGroups}
                                onChange={(e) => setNumberOfGroups(e.target.value)}
                                className={`w-14 px-2 py-1.5 text-xs font-bold rounded-lg border text-center focus:outline-none focus:ring-1 focus:ring-purple-500 ${
                                  darkMode ? 'bg-slate-800 border-slate-700' : 'bg-slate-50 border-slate-200'
                                }`}
                              />
                              <button
                                type="button"
                                onClick={handleMakeGroups}
                                className="flex-1 py-2 bg-purple-500 hover:bg-purple-600 text-white font-bold text-xs rounded-lg transition-all flex items-center justify-center gap-1.5"
                              >
                                <Layers className="w-4 h-4" />
                                <span>Топқа бөлу</span>
                              </button>
                            </div>

                            {generatedGroups ? (
                              <div className="space-y-3 mt-4 animate-fadeIn">
                                <div className="flex items-center justify-between border-b border-slate-200 dark:border-slate-800 pb-2">
                                  <h5 className="text-xs font-black uppercase text-slate-400">
                                    {generatedGroups.type === 'pairs' ? 'Құрылған Жұптар:' : 'Құрылған Топтар:'}
                                  </h5>
                                  <button onClick={() => setGeneratedGroups(null)} className="text-[10px] text-slate-400 hover:text-rose-500 font-bold transition-colors">
                                    ✕ Жабу
                                  </button>
                                </div>
                                <div className="grid grid-cols-2 gap-2 max-h-[220px] overflow-y-auto pr-1">
                                  {generatedGroups.data.map((group, idx) => (
                                    <div key={idx} className="p-3 bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-xl shadow-sm hover:border-indigo-300 transition-colors">
                                      <span className="text-[10px] font-black uppercase tracking-wider text-indigo-500 block mb-2">
                                        {generatedGroups.type === 'pairs' ? `${idx + 1}-жұп` : `${idx + 1}-топ`}
                                      </span>
                                      <div className="flex flex-col gap-1.5">
                                        {group.map((st, i) => (
                                          <span key={i} className="text-xs font-semibold bg-slate-50 dark:bg-slate-800/50 px-2.5 py-1 rounded-lg border border-slate-100 dark:border-slate-700/50 flex items-center gap-1.5">
                                            <span className="w-1 h-1 rounded-full bg-emerald-500"></span> {st}
                                          </span>
                                        ))}
                                      </div>
                                    </div>
                                  ))}
                                </div>
                              </div>
                            ) : (
                              <div className="relative rounded-2xl bg-gradient-to-tr from-indigo-600 to-purple-600 p-6 text-white text-center shadow-lg flex flex-col items-center justify-center min-h-[140px] overflow-hidden mt-2">
                                {isSpinning ? (
                                  <div className="space-y-2">
                                    <div className="text-2xl font-black tracking-wider animate-bounce">{selectedStudent}</div>
                                    <p className="text-[10px] text-indigo-100 font-bold uppercase tracking-widest animate-pulse">Айналуда...</p>
                                  </div>
                                ) : selectedStudent ? (
                                  <div className="space-y-1 animate-fadeIn">
                                    <p className="text-[10px] text-emerald-200 font-extrabold uppercase tracking-widest">Таңдалған оқушы:</p>
                                    <div className="text-3xl font-black drop-shadow-md">{selectedStudent}</div>
                                  </div>
                                ) : (
                                  <div className="space-y-1">
                                    <div className="text-lg font-bold">Тақта / Топ Нәтижесі</div>
                                    <p className="text-xs text-indigo-100">Таңдау немесе топқа бөлу нәтижесі осында шығады.</p>
                                  </div>
                                )}
                              </div>
                            )}
                          </div>
                        </div>

                        {/* 2. Сабақ Хронологиялық Таймері */}
                        <div className="p-6 rounded-3xl border bg-slate-50 dark:bg-slate-950/40 space-y-4 border-slate-200 dark:border-slate-850 shadow-sm">
                          <div className="flex items-center gap-2 text-emerald-500">
                            <Timer className="w-5 h-5" />
                            <h4 className="font-extrabold text-sm sm:text-base">Сабақ Таймері мен Пресеттер</h4>
                          </div>

                          <div className="space-y-4">
                            <div className={`p-6 rounded-2xl border text-center font-mono shadow-inner ${
                              darkMode ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200'
                            }`}>
                              <span className="text-[10px] text-slate-400 font-bold uppercase block mb-1">Қалған уақыт</span>
                              <div className="text-4xl font-black text-indigo-600 dark:text-indigo-400 tracking-wider">
                                {formatTime(timerSeconds)}
                              </div>
                            </div>

                            <div className="flex gap-2 p-2 bg-white dark:bg-slate-900 border rounded-2xl items-center justify-between border-slate-200 dark:border-slate-800">
                              <span className="text-xs font-bold text-slate-400 pl-2">Баптау:</span>
                              <div className="flex gap-1.5 items-center">
                                <input 
                                  type="number"
                                  min="1"
                                  max="120"
                                  value={customMinutes}
                                  onChange={(e) => setCustomMinutes(e.target.value)}
                                  className={`w-14 px-2 py-1 text-xs font-bold rounded-lg border text-center focus:outline-none focus:ring-1 focus:ring-emerald-500 ${
                                    darkMode ? 'bg-slate-800 border-slate-700' : 'bg-slate-50 border-slate-200'
                                  }`}
                                />
                                <span className="text-xs text-slate-400">мин</span>
                                <button
                                  type="button"
                                  onClick={handleCustomTimer}
                                  className="py-1 px-3 bg-emerald-500 hover:bg-emerald-600 text-white font-bold text-xs rounded-xl transition-all"
                                >
                                  Орнату
                                </button>
                              </div>
                            </div>

                            <div className="grid grid-cols-3 gap-2">
                              {[
                                { label: "5 мин (Басы)", value: 300 },
                                { label: "20 мин (Түсіндіру)", value: 1200 },
                                { label: "10 мин (Тәжірибе)", value: 600 }
                              ].map((preset, pIdx) => (
                                <button
                                  key={pIdx}
                                  onClick={() => {
                                    setIsTimerRunning(false);
                                    setTimerSeconds(preset.value);
                                  }}
                                  className={`p-2 rounded-xl text-[10px] font-bold border transition-all ${
                                    darkMode ? 'bg-slate-800 border-slate-700 text-slate-300 hover:bg-slate-700' : 'bg-white border-slate-200 text-slate-600 hover:bg-slate-100'
                                  }`}
                                >
                                  {preset.label}
                                </button>
                              ))}
                            </div>

                            <div className="flex gap-2">
                              <button
                                onClick={() => setIsTimerRunning(!isTimerRunning)}
                                className={`flex-1 py-3 rounded-xl font-bold text-xs text-white shadow-md transition-all flex items-center justify-center gap-1 ${
                                  isTimerRunning ? 'bg-amber-500 hover:bg-amber-600' : 'bg-emerald-500 hover:bg-emerald-600'
                                }`}
                              >
                                {isTimerRunning ? (
                                  <>
                                    <Pause className="w-4 h-4" /> Тоқтату
                                  </>
                                ) : (
                                  <>
                                    <Play className="w-4 h-4" /> Бастау
                                  </>
                                )}
                              </button>

                              <button
                                onClick={() => {
                                  setIsTimerRunning(false);
                                  setTimerSeconds(300);
                                }}
                                className={`py-3 px-4 rounded-xl font-bold text-xs border transition-all flex items-center justify-center ${
                                  darkMode ? 'bg-slate-800 border-slate-700 text-slate-300 hover:bg-slate-700' : 'bg-slate-100 border-slate-200 text-slate-600 hover:bg-slate-200'
                                }`}
                                title="Қайта қою"
                              >
                                <RotateCcw className="w-4 h-4" />
                              </button>
                            </div>

                          </div>
                        </div>
                      </div>
                    </div>
                  )}

                </div>

              </div>
            )}

          </div>

        </div>

      </main>

      {/* Ақпараттық Футер */}
      <footer className={`mt-20 border-t py-8 text-center transition-colors ${darkMode ? 'bg-slate-950 border-slate-900 text-slate-500' : 'bg-white border-slate-100 text-slate-400'}`}>
        <p className="text-xs font-medium">© 2026 ҰстазКөмекшісі Платформасы. Барлық құқықтар қорғалған.</p>
        <p className="text-[10px] mt-1 text-slate-400">Сабақ дайындау құралы мұғалімдердің кәсіби дамуы мен жұмысты цифрландыру мақсатында әзірленді.</p>
      </footer>

      {/* TOLЫҚ ЭКРАНДЫ СЛАЙД ТЕАТРЫ РЕЖИМІ */}
      {isSlideFullscreen && generatedData && (
        <div className="fixed inset-0 z-50 bg-slate-950 flex flex-col justify-between p-6 sm:p-12 text-slate-100 select-none animate-fadeIn">
          <div className="flex justify-between items-center border-b border-slate-800 pb-4">
            <div className="flex items-center gap-2">
              <span className="text-emerald-500 text-xl font-black">●</span>
              <span className="text-sm font-extrabold tracking-widest text-slate-400 uppercase">
                {generatedData.lessonPlan.subject} • {generatedData.lessonPlan.grade}
              </span>
            </div>
            <div className="flex items-center gap-4">
              <button
                onClick={() => speakText(`${generatedData.slides[currentSlideIndex].title}. ${generatedData.slides[currentSlideIndex].bullets.join('. ')}`)}
                className="p-3 bg-slate-900 hover:bg-slate-800 text-slate-300 rounded-2xl border border-slate-800 transition-colors"
                title="Дауыстап оқу"
              >
                <Volume2 className="w-5 h-5" />
              </button>
              <button
                onClick={() => setIsSlideFullscreen(false)}
                className="p-3 bg-rose-950/40 hover:bg-rose-900/40 text-rose-400 rounded-2xl border border-rose-900/30 transition-colors flex items-center gap-1.5 font-bold text-xs"
                title="Экраннан шығу"
              >
                <Minimize2 className="w-5 h-5" />
                <span>Жабу</span>
              </button>
            </div>
          </div>

          {(() => {
            const currentSlide = generatedData.slides[currentSlideIndex];
            const theme = getSlideTheme(generatedData.lessonPlan.subject);
            return (
              <div className="grid grid-cols-1 lg:grid-cols-12 gap-12 items-center my-auto max-w-6xl mx-auto w-full">
                <div className="lg:col-span-4 flex justify-center">
                  <div className="w-64 h-64 bg-slate-900 border-2 border-indigo-500/30 rounded-3xl flex flex-col items-center justify-center p-6 text-center shadow-2xl relative overflow-hidden">
                    <div className="text-8xl mb-4 filter drop-shadow-2xl">{theme.icon}</div>
                    <span className="text-xs font-bold text-indigo-400 uppercase tracking-widest">{generatedData.lessonPlan.title}</span>
                  </div>
                </div>
                <div className="lg:col-span-8 space-y-6">
                  <span className="text-xs font-black tracking-widest text-indigo-400 uppercase">Слайд #{currentSlide.slideNum}</span>
                  <h2 className="text-3xl sm:text-4xl font-black tracking-tight leading-tight">{currentSlide.title}</h2>
                  <ul className="space-y-4 pt-2">
                    {currentSlide.bullets.map((bullet, idx) => (
                      <li key={idx} className="flex items-start gap-4 text-base sm:text-lg font-bold text-slate-300 leading-relaxed">
                        <span className="bg-emerald-500 text-slate-950 w-7 h-7 rounded-full flex items-center justify-center shrink-0 font-extrabold text-sm mt-0.5">✓</span>
                        <span>{bullet}</span>
                      </li>
                    ))}
                  </ul>
                </div>
              </div>
            );
          })()}

          <div className="flex justify-between items-center border-t border-slate-800 pt-6">
            <button
              onClick={() => setCurrentSlideIndex(prev => Math.max(0, prev - 1))}
              disabled={currentSlideIndex === 0}
              className={`py-3 px-6 rounded-2xl font-bold text-sm transition-all border ${
                currentSlideIndex === 0 
                  ? 'border-slate-900 bg-slate-950 text-slate-700 cursor-not-allowed' 
                  : 'border-slate-800 bg-slate-900 text-slate-300 hover:bg-slate-800'
              }`}
            >
              ← Алдыңғы слайд
            </button>
            <div className="flex gap-2">
              {generatedData.slides.map((_, idx) => (
                <button
                  key={idx}
                  onClick={() => setCurrentSlideIndex(idx)}
                  className={`h-3 rounded-full transition-all duration-300 ${
                    idx === currentSlideIndex ? 'w-8 bg-emerald-500' : 'w-3 bg-slate-800'
                  }`}
                ></button>
              ))}
            </div>
            <button
              onClick={() => setCurrentSlideIndex(prev => Math.min(generatedData.slides.length - 1, prev + 1))}
              disabled={currentSlideIndex === generatedData.slides.length - 1}
              className={`py-3 px-6 rounded-2xl font-bold text-sm transition-all border ${
                currentSlideIndex === generatedData.slides.length - 1 
                  ? 'border-slate-900 bg-slate-950 text-slate-700 cursor-not-allowed' 
                  : 'border-slate-800 bg-slate-900 text-slate-300 hover:bg-slate-800'
              }`}
            >
              Келесі слайд →
            </button>
          </div>
        </div>
      )}

    </div>
  );
}
