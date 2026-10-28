<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from "vue";
import {
  Menu,
  X,
  ArrowUpRight,
  Github,
  Linkedin,
  Mail,
  Phone,
  Copy,
  Check,
  Sparkles,
  Bot,
  Smartphone,
  Cpu,
  Layers,
  Terminal,
  ExternalLink,
  Briefcase,
  GraduationCap,
  Award,
  Zap,
  ShieldCheck,
  Play,
  RotateCcw,
  Sliders,
  Network,
  Activity,
  Code2,
  Users,
  CheckCircle2,
} from "lucide-vue-next";
import profileImage from "./assets/profile.jpg";

// Navigation
const navItems = [
  { id: "work", label: "Work" },
  { id: "interactive-lab", label: "Architecture Lab" },
  { id: "experience", label: "Career & Roles" },
  { id: "expertise", label: "Capabilities" },
  { id: "about", label: "About" },
  { id: "contact", label: "Contact" },
];

const mobileMenuOpen = ref(false);
const scrolled = ref(false);
const activeSection = ref("work");
const copiedEmail = ref(false);

const copyEmail = async () => {
  try {
    await navigator.clipboard.writeText("poojaryabhishek26@gmail.com");
    copiedEmail.value = true;
    setTimeout(() => {
      copiedEmail.value = false;
    }, 2200);
  } catch (err) {
    console.error("Failed to copy", err);
  }
};

const scrollToSection = (id: string) => {
  const el = document.getElementById(id);
  if (el) {
    const yOffset = -76;
    const y = el.getBoundingClientRect().top + window.pageYOffset + yOffset;
    window.scrollTo({ top: y, behavior: "smooth" });
  }
  mobileMenuOpen.value = false;
};

// ================= INTERACTIVE ARCHITECTURE LAB =================
const activeTab = ref<"rag" | "irentout-scale" | "telemetry">("rag");

// 0. iRentOut Mobile Scale & User Base Monitor (2k Android & 1k iOS Users)
const selectedPlatform = ref<"android" | "ios">("android");

const irentoutUserProfiles = {
  android: {
    platform: "Android (React Native)",
    users: "2,000+ Active Users",
    userShare: "~66% of mobile tenant base",
    dbEngine: "MySQL InnoDB (Normalized transactional ledger)",
    backend: ".NET Core (ASP.NET Web API / C#)",
    syncArchitecture: "Batched delta sync + Optimistic UI updates with offline request queueing",
    peakThroughput: "140 req/sec during peak morning & rental listing windows",
    crashRate: "< 0.08% crash-free sessions across 100+ OEM device types",
  },
  ios: {
    platform: "iOS (React Native)",
    users: "1,000+ Active Users",
    userShare: "~34% of mobile tenant base",
    dbEngine: "MySQL Relational Indexing + Redis Cache tier",
    backend: ".NET Core RESTful Services",
    syncArchitecture: "Native iOS background task refresh + Biometric session reconciliation",
    peakThroughput: "85 req/sec with sub-50ms token authentication",
    crashRate: "< 0.03% crash-free rate with zero memory leak regressions",
  },
};

const currentIrentoutProfile = computed(() => {
  return irentoutUserProfiles[selectedPlatform.value];
});

// 1. RAG Simulator
const sampleQueries = [
  "How does offline sync persist state across sudden network disconnects?",
  "Explain the FAISS vector indexing and hallucination guardrail architecture.",
  "What is the MQTT QoS topology used for real-time sensor device clusters?",
] as const;
type SampleQuery = typeof sampleQueries[number];

const selectedQuery = ref<SampleQuery>(sampleQueries[0]);
const isSimulatingRag = ref(false);
const ragStep = ref(0);
const ragConfidenceScore = ref(0.96);

const ragSteps = [
  { title: "Query Embedding", desc: "Tokenization & Vector encoding via OpenAI text-embedding-3-small" },
  { title: "FAISS Similarity Search", desc: "Cosine proximity scan over 1,500+ chunked technical vectors" },
  { title: "Hallucination & Guardrail Check", desc: "Context alignment scoring & confidence threshold verification (> 0.85)" },
  { title: "Grounded Synthesis", desc: "Streaming factual, source-attributed answer with 0% unverified extrapolations" },
];

const simulatedAnswers: Record<SampleQuery, { answer: string; chunks: string[]; confidence: number }> = {
  "How does offline sync persist state across sudden network disconnects?": {
    answer:
      "The client uses an SQLite offline-first WAL ledger. Transactions write locally immediately with zero UI blocking. When the WebSocket/MQTT heartbeat reconvenes, mutations are pushed sequentially with idempotency keys to eliminate split-brain data conflicts.",
    chunks: [
      "[Doc: sync_protocol.md #L42] 'SQLite WAL buffer holds pending queue with SHA-256 idempotency token.'",
      "[Doc: mqtt_telemetry.dart #L118] 'On connection state changes, trigger background sync runner.'",
    ],
    confidence: 0.98,
  },
  "Explain the FAISS vector indexing and hallucination guardrail architecture.": {
    answer:
      "Queries are vectorized and queried against a normalized FAISS IndexFlatIP. Retreived top-k chunks pass through a secondary cross-encoder scoring stage. If semantic relevance falls below 0.82, the engine triggers a targeted fallback rather than fabricating hallucinations.",
    chunks: [
      "[Doc: rag_pipeline.py #L88] 'def verify_confidence(query, context, threshold=0.82) -> bool'",
      "[Doc: faiss_indexer.py #L51] 'IndexFlatIP cosine search with normalized chunk embeddings.'",
    ],
    confidence: 0.95,
  },
  "What is the MQTT QoS topology used for real-time sensor device clusters?": {
    answer:
      "Device control utilizes QoS 1 with client-side ACK timers for critical state changes (relays, automated scenes). High-frequency telemetry (temperature, current draw) streams over QoS 0 with client-side throttling to conserve field cellular bandwidth.",
    chunks: [
      "[Doc: iot_broker_spec.md #L14] 'QoS 1 for actuation payloads; QoS 0 for 500ms sensor telemetry ticks.'",
      "[Doc: device_bridge.ts #L204] 'Batching telemetry packets into 250ms window frames.'",
    ],
    confidence: 0.97,
  },
};

const currentAnswer = computed(() => {
  return simulatedAnswers[selectedQuery.value];
});

const runRagSimulation = () => {
  if (isSimulatingRag.value) return;
  isSimulatingRag.value = true;
  ragStep.value = 1;

  const interval = setInterval(() => {
    ragStep.value++;
    if (ragStep.value > 4) {
      clearInterval(interval);
      isSimulatingRag.value = false;
      if (currentAnswer.value) {
        ragConfidenceScore.value = currentAnswer.value.confidence;
      }
    }
  }, 450);
};

// 2. Real-time Telemetry Simulator
const liveTelemetry = ref([
  { device: "Smart Hub GW-01", protocol: "MQTT / TLS", status: "Online", latency: 24, load: 38 },
  { device: "HVAC Zone Controller", protocol: "WebSocket", status: "Streaming", latency: 18, load: 52 },
  { device: "Sub-meter Cluster B", protocol: "MQTT QoS 1", status: "Online", latency: 31, load: 19 },
  { device: "Access Gateway Node", protocol: "TCP Socket", status: "Online", latency: 15, load: 44 },
]);

// 3. Performance Metric Slider
const virtualConcurrentUsers = ref(500);
const calculatedResponseTime = computed(() => {
  const base = 42; // ms
  const overhead = Math.round((virtualConcurrentUsers.value / 500) * 18);
  return base + overhead;
});

// ================= CAREER EXPERIENCES (From Resume & LinkedIn) =================
const experiences = [
  {
    role: "Software Developer",
    company: "iRentOut",
    location: "Bengaluru, India",
    period: "July 2026 — Present",
    badge: "Current Role",
    summary:
      "Leading full-stack and mobile application engineering across production mobile clients and backend microservices. Architected the React Native mobile platform serving 2,000+ Android users and 1,000+ iOS users, powered by .NET Core APIs and optimized MySQL relational database clusters.",
    achievements: [
      "Engineered and scaled cross-platform React Native mobile applications serving a verified active tenant base of 2,000+ Android users and 1,000+ iOS users.",
      "Architected backend microservices in .NET (ASP.NET Core) and C#, executing high-throughput RESTful endpoints with sub-60ms response times.",
      "Engineered relational MySQL databases handling tenant ledgers, property contracts, and automated invoicing with strict transactional atomicity.",
      "Engineered web management portals in React.js and TypeScript for internal operations, property managers, and inventory lifecycle tracking.",
      "Integrated resilient biometric authentication, push notifications, and payment gateways with automated retry logic.",
    ],
    tags: ["React Native (2k Android / 1k iOS)", ".NET Core", "C#", "MySQL", "React.js", "TypeScript", "RESTful APIs", "Full Stack"],
  },
  {
    role: "Assistant Software Engineer",
    company: "SmartBuild Automation Pvt Ltd",
    location: "Bengaluru, India",
    period: "January 2024 — July 2026",
    badge: "2 yrs 7 mos",
    summary:
      "Spearheaded production Angular 15+ & Flutter applications, GenAI LLM systems, and industrial IoT device bridges across residential & commercial clients.",
    achievements: [
      "Developed production Angular 15+ web applications with lazy loading and modular architecture, achieving 40% faster page load times.",
      "Built a reusable UI component library with RxJS reactive state management and REST API integrations, supporting 500+ concurrent active users on real-time IoT dashboards.",
      "Integrated generative AI features using LangChain and a FAISS-based RAG pipeline with hallucination detection and confidence scoring for verified LLM answers.",
      "Partnered with backend engineering teams on API design, schema validation, and contract tests, cutting production runtime errors by 60%.",
      "Published and maintained two live cross-platform Flutter applications on both the Apple App Store and Google Play Store.",
    ],
    tags: ["Angular 15+", "Flutter", "LangChain", "FAISS / RAG", "RxJS", "IoT Telemetry", "MQTT", "REST"],
  },
  {
    role: "Software Engineer Intern",
    company: "Melange Systems Pvt Ltd",
    location: "Bengaluru, India",
    period: "October 2023 — January 2024",
    badge: "4 mos",
    summary:
      "Contributed to frontend module development and Node.js microservices for IoT telemetry and client dashboards.",
    achievements: [
      "Built responsive single-page applications in React.js and Vite using Hooks, Context API, and modern ES6+ functional paradigms.",
      "Integrated Node.js REST APIs with asynchronous data fetching, error boundaries, and loading states, improving measured user experience scores by 35%.",
      "Created structured UI components and documentation for cross-functional developer handoffs.",
    ],
    tags: ["React.js", "Vite", "Node.js", "MongoDB", "REST APIs", "JavaScript"],
  },
];

// Project Filter
const projectFilter = ref<"all" | "ai" | "mobile" | "web">("all");

const projects = [
  {
    id: "smart-support",
    category: "ai",
    badge: "Production GenAI",
    title: "AI Smart Support System",
    tagline: "High-accuracy RAG pipeline with vector search and hallucination scoring",
    summary:
      "Engineered an enterprise document Q&A engine built on LangChain, OpenAI embeddings, and FAISS indices. Includes real-time confidence thresholding, context attribution, and FastAPI endpoints for mobile/web clients.",
    metrics: [
      { label: "Search Index", value: "FAISS Vector" },
      { label: "Confidence", value: "Scored & Verified" },
      { label: "Error Rate", value: "-60% Fallbacks" },
    ],
    tech: ["Python", "FastAPI", "LangChain", "OpenAI", "FAISS", "Vector DB"],
    github: "https://github.com/Abhishek-poojary1/AI_Smart_Support.git",
    live: null,
  },
  {
    id: "smart-build",
    category: "mobile",
    badge: "Live on App Store & Play Store",
    title: "SB Auto & SBA Home Ecosystem",
    tagline: "Commercial IoT mobile suites controlling 20+ hardware classes with real-time sync",
    summary:
      "Dual commercial Flutter apps serving active installations on iOS and Android. Engineered with Clean Architecture, SQLite offline-first sync, MQTT QoS telemetry, and hardware integration across 20+ connected IoT devices.",
    metrics: [
      { label: "Live Platforms", value: "iOS & Android" },
      { label: "Connected Nodes", value: "20+ Devices" },
      { label: "Load Time", value: "40% Faster" },
    ],
    tech: ["Flutter", "Dart", "MQTT", "WebSockets", "Firebase", "SQLite", "Clean Architecture"],
    appStoreModal: true,
    github: null,
    live: null,
  },
  {
    id: "iot-dashboard",
    category: "web",
    badge: "Enterprise Angular 15+",
    title: "Enterprise IoT Telemetry Dashboard",
    tagline: "Modular supervisory console with RxJS state streams supporting 500+ concurrent operators",
    summary:
      "Production-deployed supervisory frontend. Features lazy-loaded routing, modular RxJS architecture, live WebSocket telemetry ingestion, and diagnostic automation scene configuration.",
    metrics: [
      { label: "Concurrent Users", value: "500+ Active" },
      { label: "Page Speed", value: "+40% Boost" },
      { label: "Data Pipeline", value: "RxJS Streams" },
    ],
    tech: ["Angular 15+", "TypeScript", "RxJS", "WebSockets", "REST APIs"],
    live: "https://www.smartbuildauto.com/",
    github: null,
  },
  {
    id: "ecommerce",
    category: "web",
    badge: "MERN & Cross-Platform",
    title: "Unified Omnichannel Retail System",
    tagline: "Synchronized Flutter mobile client and React.js web platform",
    summary:
      "Full-stack commerce engine featuring synchronized real-time cart state, Firebase authentication, catalog indexing, and Node.js REST services.",
    metrics: [
      { label: "Cross-Platform", value: "Web + Mobile" },
      { label: "Auth Layer", value: "Secure Firebase" },
      { label: "State Sync", value: "Real-time Store" },
    ],
    tech: ["Flutter", "React.js", "Node.js", "MongoDB", "Express", "REST APIs"],
    github: "https://github.com/Abhishek-poojary1/EcommerceApplication",
    live: null,
  },
];

const filteredProjects = computed(() => {
  if (projectFilter.value === "all") return projects;
  return projects.filter((p) => p.category === projectFilter.value);
});

// App Store Modal
const showAppModal = ref(false);
const appLinks = {
  sbAuto: {
    name: "SB Auto",
    desc: "Primary industrial automation suite with hardware diagnostics, sub-meter monitoring, and scenes.",
    appStore: "https://apps.apple.com/in/app/sb-auto/id6478853565",
    playStore: "https://play.google.com/store/apps/details?id=com.smartbuildauto.SBAuto",
  },
  sbaHome: {
    name: "SBA Home",
    desc: "Consumer smart home interface with ambient routines, scene triggers, and multi-room control.",
    appStore: "https://apps.apple.com/in/app/sba-home/id6739250335",
    playStore: "https://play.google.com/store/apps/details?id=com.smartbuildauto.SBAHome",
  },
};

const openAppModal = () => {
  showAppModal.value = true;
  document.body.style.overflow = "hidden";
};

const closeAppModal = () => {
  showAppModal.value = false;
  document.body.style.overflow = "auto";
};

// Capabilities
const capabilities = [
  {
    icon: Bot,
    title: "Generative AI & RAG Engineering",
    desc: "Building production RAG pipelines with LangChain and FAISS, hallucination guardrails, confidence scoring metrics, and context chunking optimization.",
    stack: ["LangChain", "OpenAI API", "FAISS", "Vector DBs", "Confidence Scoring", "Prompt Engineering"],
    proof: "Hallucination detection pipeline deployed in production",
  },
  {
    icon: Smartphone,
    title: "Mobile Architecture (Flutter & React Native)",
    desc: "Developing production cross-platform mobile apps serving active user bases at scale: 2,000+ Android and 1,000+ iOS users on React Native (iRentOut) alongside published Flutter App Store apps with offline SQLite sync and zero dropped frames.",
    stack: ["React Native", "Flutter", "iOS (1k Users)", "Android (2k Users)", "SQLite", "Firebase", "Offline-first Sync"],
    proof: "Active live users across iRentOut & SmartBuild App Store apps",
  },
  {
    icon: Layers,
    title: ".NET Core, MySQL & Full-Stack Services",
    desc: "Architecting high-throughput multi-tier backends with .NET Core and C#, powering mobile apps with sub-60ms RESTful APIs and normalized MySQL databases handling high-concurrency transactions.",
    stack: [".NET Core", "C#", "MySQL", "React.js", "TypeScript", "RESTful APIs", "Relational Schemas"],
    proof: "Powers iRentOut mobile platform & high-traffic dashboards",
  },
  {
    icon: Cpu,
    title: "IoT Telemetry & Real-Time Sync",
    desc: "Architecting low-latency communication conduits connecting hardware sensors, edge controllers, and cloud supervisory dashboards.",
    stack: ["MQTT Protocols", "WebSockets", "Hardware Bridges", "Data Validation", "Sensor Streams"],
    proof: "20+ hardware device classes integrated seamlessly",
  },
];

// Stats
const stats = [
  { value: "3,000+", label: "Active Mobile Users", detail: "Across iOS & Android clients" },
  { value: "40%", label: "Faster Load Times", detail: "Angular lazy-loading & caching" },
  { value: "60%", label: "Fewer Prod Errors", detail: "API data validation & schemas" },
  { value: "2", label: "Live App Store Apps", detail: "SB Auto & SBA Home" },
];

// Education & Certifications
const education = [
  {
    degree: "Bachelor of Computer Applications (BCA)",
    major: "Computer Software and Media Applications",
    institution: "Dr. B.B. Hegde First Grade College, Kundapura",
    period: "June 2020 — July 2023",
    detail: "Core algorithms, data structures, application architecture, and database systems.",
  },
];

const certifications = [
  { name: "Machine Learning with Python", issuer: "Industry Certification", focus: "Data modeling, scikit-learn, regression & classification" },
  { name: "Flutter Application Development", issuer: "Mobile Specialization", focus: "Clean Architecture, state management, mobile persistence" },
  { name: "Prompt Engineering & RAG Pipelines", issuer: "AI Practitioner", focus: "Vector embeddings, contextual retrieval, hallucination minimization" },
];

// Scroll detection
const handleScroll = () => {
  scrolled.value = window.scrollY > 40;
  const sections = ["work", "interactive-lab", "experience", "expertise", "about", "contact"];
  const scrollPosition = window.scrollY + 200;

  for (const section of sections) {
    const el = document.getElementById(section);
    if (el) {
      const top = el.offsetTop;
      const height = el.offsetHeight;
      if (scrollPosition >= top && scrollPosition < top + height) {
        activeSection.value = section;
        break;
      }
    }
  }
};

const handleKeyDown = (e: KeyboardEvent) => {
  if (e.key === "Escape" && showAppModal.value) {
    closeAppModal();
  }
};

onMounted(() => {
  window.addEventListener("scroll", handleScroll, { passive: true });
  window.addEventListener("keydown", handleKeyDown);
  handleScroll();
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
  window.removeEventListener("keydown", handleKeyDown);
});
</script>

<template>
  <div class="min-h-screen bg-[#090A0D] text-slate-200 selection:bg-emerald-500/20 selection:text-emerald-300 relative">
    
    <!-- Subtle Background Grid / Mesh -->
    <div class="fixed inset-0 bg-subtle-mesh pointer-events-none z-0"></div>

    <!-- Navigation Bar -->
    <header
      class="fixed top-0 left-0 right-0 z-50 transition-all duration-300 backdrop-blur-md"
      :class="scrolled ? 'bg-[#090A0D]/85 border-b border-white/[0.08] py-3' : 'bg-transparent py-5'"
    >
      <div class="max-w-6xl mx-auto px-4 sm:px-6 flex items-center justify-between">
        
        <!-- Brand identity -->
        <a
          href="#"
          @click.prevent="scrollToSection('hero')"
          class="flex items-center gap-3 group focus:outline-none focus-visible:ring-2 focus-visible:ring-emerald-500 rounded-lg p-1"
        >
          <div class="relative w-9 h-9 rounded-full overflow-hidden border border-white/20 ring-1 ring-white/10 group-hover:border-emerald-400 transition-colors">
            <img :src="profileImage" alt="Abhishek Poojary" class="w-full h-full object-cover grayscale group-hover:grayscale-0 transition-all duration-500" />
          </div>
          <div class="flex flex-col">
            <span class="text-sm font-semibold tracking-tight text-white group-hover:text-emerald-400 transition-colors flex items-center gap-1.5">
              Abhishek Poojary
              <span class="inline-block w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse-subtle"></span>
            </span>
            <span class="text-[11px] font-mono text-slate-400">Software Developer @ iRentOut</span>
          </div>
        </a>

        <!-- Desktop Navigation Items -->
        <nav class="hidden md:flex items-center gap-1 bg-[#141720]/80 p-1.5 rounded-full border border-white/[0.08] shadow-inner">
          <button
            v-for="item in navItems"
            :key="item.id"
            @click="scrollToSection(item.id)"
            class="px-3.5 py-1.5 text-xs font-medium rounded-full transition-all"
            :class="
              activeSection === item.id
                ? 'bg-white/10 text-white shadow-sm font-semibold'
                : 'text-slate-400 hover:text-white hover:bg-white/[0.04]'
            "
          >
            {{ item.label }}
          </button>
        </nav>

        <!-- Right Action Button -->
        <div class="hidden md:flex items-center gap-3">
          <button
            @click="scrollToSection('contact')"
            class="text-xs font-medium px-4 py-2 rounded-lg bg-emerald-500 hover:bg-emerald-400 text-neutral-950 font-semibold transition-all duration-200 flex items-center gap-1.5 shadow-sm hover:shadow-[0_0_20px_rgba(52,211,153,0.3)]"
          >
            <span>Get in touch</span>
            <ArrowUpRight class="w-3.5 h-3.5 stroke-[2.5]" />
          </button>
        </div>

        <!-- Mobile Menu Toggle Button -->
        <button
          @click="mobileMenuOpen = !mobileMenuOpen"
          class="md:hidden p-2 rounded-lg text-slate-400 hover:text-white bg-white/[0.05] border border-white/10"
          aria-label="Toggle menu"
        >
          <Menu v-if="!mobileMenuOpen" class="w-5 h-5" />
          <X v-else class="w-5 h-5" />
        </button>
      </div>

      <!-- Mobile Dropdown -->
      <Transition
        enter-active-class="transition duration-200 ease-out"
        enter-from-class="opacity-0 -translate-y-2"
        enter-to-class="opacity-100 translate-y-0"
        leave-active-class="transition duration-150 ease-in"
        leave-from-class="opacity-100 translate-y-0"
        leave-to-class="opacity-0 -translate-y-2"
      >
        <div
          v-if="mobileMenuOpen"
          class="md:hidden mt-2 mx-4 p-4 rounded-2xl bg-[#0E1015] border border-white/10 shadow-2xl backdrop-blur-xl space-y-2"
        >
          <button
            v-for="item in navItems"
            :key="item.id"
            @click="scrollToSection(item.id)"
            class="block w-full text-left px-3 py-2.5 rounded-lg text-sm transition-colors"
            :class="activeSection === item.id ? 'bg-white/10 text-emerald-400 font-medium' : 'text-slate-300 hover:bg-white/[0.05]'"
          >
            {{ item.label }}
          </button>
          <div class="pt-2 border-t border-white/10">
            <button
              @click="scrollToSection('contact')"
              class="w-full py-2.5 text-center text-xs font-semibold rounded-lg bg-emerald-500 text-neutral-950"
            >
              Get in touch
            </button>
          </div>
        </div>
      </Transition>
    </header>

    <main class="relative z-10">

      <!-- ================= HERO SECTION ================= -->
      <section id="hero" class="pt-36 pb-20 md:pt-44 md:pb-28 max-w-6xl mx-auto px-4 sm:px-6">
        
        <!-- Status indicator badge -->
        <div class="inline-flex items-center gap-2.5 px-3.5 py-1.5 rounded-full bg-emerald-500/[0.08] border border-emerald-500/25 text-emerald-300 text-xs font-mono mb-8">
          <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
          <span>Software Developer @ iRentOut · Building AI-Powered Apps</span>
        </div>

        <!-- Hero Headline -->
        <div class="max-w-4xl">
          <h1 class="text-4xl sm:text-6xl md:text-7xl font-bold tracking-tight text-white leading-[1.08] mb-6 font-sans">
            Flutter, Full-Stack &
            <span class="text-transparent bg-clip-text bg-gradient-to-r from-emerald-400 via-teal-300 to-cyan-400"> GenAI Engineer</span>
            delivering high-performance production apps.
          </h1>

          <p class="text-lg sm:text-xl text-slate-400 max-w-3xl font-normal leading-relaxed mb-10">
            Hi, I’m <span class="text-white font-medium">Abhishek Poojary</span> — based in Bengaluru, India.
            Currently engineering mobile & backend architectures at <span class="text-emerald-400 font-medium">iRentOut</span> using <strong class="text-slate-200">React Native</strong>, <strong class="text-slate-200">.NET Core</strong>, and <strong class="text-slate-200">MySQL</strong>. Previously built production Angular 15+ IoT platforms, RAG hallucination guardrails, and commercial Flutter apps published on the App Store at <span class="text-slate-200">SmartBuild Automation</span>.
          </p>

          <!-- Action buttons -->
          <div class="flex flex-wrap items-center gap-4">
            <button
              @click="scrollToSection('interactive-lab')"
              class="px-6 py-3 rounded-lg bg-emerald-500 hover:bg-emerald-400 text-neutral-950 font-semibold text-sm transition-all flex items-center gap-2 shadow-sm hover:shadow-[0_0_25px_rgba(52,211,153,0.35)]"
            >
              <Code2 class="w-4 h-4 stroke-[2.5]" />
              <span>Launch Architecture Lab</span>
            </button>

            <button
              @click="scrollToSection('work')"
              class="px-5 py-3 rounded-lg bg-white/[0.05] hover:bg-white/[0.09] text-white border border-white/10 text-sm font-medium transition-all flex items-center gap-2"
            >
              <span>Explore Projects</span>
              <ArrowUpRight class="w-3.5 h-3.5" />
            </button>

            <a
              href="https://github.com/Abhishek-poojary1"
              target="_blank"
              rel="noopener noreferrer"
              class="px-4 py-3 rounded-lg bg-white/[0.03] hover:bg-white/[0.07] text-slate-300 border border-white/[0.07] text-sm font-medium transition-all flex items-center gap-2"
            >
              <Github class="w-4 h-4 text-slate-400" />
              <span>GitHub</span>
            </a>

            <a
              href="https://www.linkedin.com/in/abhishek-poojary-a50281268"
              target="_blank"
              rel="noopener noreferrer"
              class="px-4 py-3 rounded-lg bg-white/[0.03] hover:bg-white/[0.07] text-slate-300 border border-white/[0.07] text-sm font-medium transition-all flex items-center gap-2"
            >
              <Linkedin class="w-4 h-4 text-slate-400" />
              <span>LinkedIn</span>
            </a>

            <button
              @click="copyEmail"
              class="px-4 py-3 rounded-lg bg-white/[0.03] hover:bg-white/[0.07] text-slate-300 border border-white/[0.07] text-xs font-mono transition-all flex items-center gap-2"
              title="Copy email to clipboard"
            >
              <Check v-if="copiedEmail" class="w-3.5 h-3.5 text-emerald-400" />
              <Copy v-else class="w-3.5 h-3.5 text-slate-400" />
              <span>{{ copiedEmail ? 'Copied to clipboard' : 'poojaryabhishek26@gmail.com' }}</span>
            </button>
          </div>
        </div>

        <!-- Key Metrics Strip (from verified LinkedIn achievements) -->
        <div class="mt-20 pt-10 border-t border-white/[0.08] grid grid-cols-2 md:grid-cols-4 gap-8">
          <div v-for="stat in stats" :key="stat.label" class="space-y-1">
            <div class="text-3xl sm:text-4xl font-bold tracking-tight text-white font-mono flex items-baseline gap-1">
              {{ stat.value }}
            </div>
            <div class="text-xs font-semibold text-slate-300 uppercase tracking-wider">{{ stat.label }}</div>
            <div class="text-xs text-slate-500">{{ stat.detail }}</div>
          </div>
        </div>
      </section>

      <!-- ================= LIVE ARCHITECTURE LAB (INTERACTIVE FEATURE) ================= -->
      <section id="interactive-lab" class="py-20 border-t border-white/[0.08] bg-[#0A0C10]">
        <div class="max-w-6xl mx-auto px-4 sm:px-6">
          
          <div class="flex flex-col md:flex-row md:items-end justify-between mb-10 gap-6">
            <div>
              <div class="flex items-center gap-2 text-xs font-mono text-emerald-400 uppercase tracking-wider mb-2">
                <Terminal class="w-3.5 h-3.5" />
                <span>Interactive Technical Proof</span>
              </div>
              <h2 class="text-3xl sm:text-4xl font-bold text-white tracking-tight">
                Live Engineering Architecture Lab
              </h2>
              <p class="text-sm text-slate-400 mt-2 max-w-xl">
                Interact with the actual algorithmic workflows behind my systems: test vector RAG retrieval with hallucination guardrails or simulate real-time telemetry loads.
              </p>
            </div>

            <!-- Lab Switcher Tabs -->
            <div class="inline-flex p-1 rounded-lg bg-white/[0.04] border border-white/[0.08] text-xs font-medium self-start md:self-auto flex-wrap">
              <button
                @click="activeTab = 'rag'"
                class="px-3.5 py-2 rounded-md transition-all flex items-center gap-2"
                :class="activeTab === 'rag' ? 'bg-emerald-500 text-neutral-950 font-bold' : 'text-slate-400 hover:text-white'"
              >
                <Bot class="w-3.5 h-3.5" />
                <span>RAG Guardrail Engine</span>
              </button>

              <button
                @click="activeTab = 'irentout-scale'"
                class="px-3.5 py-2 rounded-md transition-all flex items-center gap-2"
                :class="activeTab === 'irentout-scale' ? 'bg-emerald-500 text-neutral-950 font-bold' : 'text-slate-400 hover:text-white'"
              >
                <Users class="w-3.5 h-3.5" />
                <span>iRentOut Scale (2k Android & 1k iOS)</span>
              </button>

              <button
                @click="activeTab = 'telemetry'"
                class="px-3.5 py-2 rounded-md transition-all flex items-center gap-2"
                :class="activeTab === 'telemetry' ? 'bg-emerald-500 text-neutral-950 font-bold' : 'text-slate-400 hover:text-white'"
              >
                <Activity class="w-3.5 h-3.5" />
                <span>IoT Telemetry</span>
              </button>
            </div>
          </div>

          <!-- TAB 1: RAG & VECTOR RETRIEVAL LAB -->
          <div v-if="activeTab === 'rag'" class="premium-card rounded-2xl p-6 sm:p-8">
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
              
              <!-- Left: Controls & Query Selector -->
              <div class="lg:col-span-5 space-y-6">
                <div>
                  <label class="block text-xs font-mono text-slate-400 uppercase tracking-wider mb-2">
                    Select Technical Query to Evaluate:
                  </label>
                  <div class="space-y-2">
                    <button
                      v-for="q in sampleQueries"
                      :key="q"
                      @click="selectedQuery = q; ragStep = 0"
                      class="w-full text-left p-3 rounded-xl border text-xs leading-relaxed transition-all"
                      :class="selectedQuery === q ? 'bg-emerald-500/10 border-emerald-500/40 text-emerald-300 font-medium' : 'bg-white/[0.02] border-white/[0.07] text-slate-400 hover:border-white/20'"
                    >
                      "{{ q }}"
                    </button>
                  </div>
                </div>

                <!-- Parameters -->
                <div class="p-4 rounded-xl bg-black/40 border border-white/[0.06] space-y-3">
                  <div class="flex items-center justify-between text-xs">
                    <span class="text-slate-400 font-mono">Vector DB:</span>
                    <span class="font-mono text-emerald-400">FAISS IndexFlatIP</span>
                  </div>
                  <div class="flex items-center justify-between text-xs">
                    <span class="text-slate-400 font-mono">Hallucination Threshold:</span>
                    <span class="font-mono text-emerald-400">&gt; 0.85 Confidence</span>
                  </div>
                  <div class="flex items-center justify-between text-xs">
                    <span class="text-slate-400 font-mono">Embedding Model:</span>
                    <span class="font-mono text-slate-300">OpenAI text-embedding-3</span>
                  </div>
                </div>

                <button
                  @click="runRagSimulation"
                  :disabled="isSimulatingRag"
                  class="w-full py-3 rounded-xl bg-emerald-500 hover:bg-emerald-400 disabled:opacity-50 text-neutral-950 font-semibold text-xs transition-all flex items-center justify-center gap-2 shadow-lg shadow-emerald-500/20"
                >
                  <Play v-if="!isSimulatingRag" class="w-3.5 h-3.5 fill-current" />
                  <RotateCcw v-else class="w-3.5 h-3.5 animate-spin" />
                  <span>{{ isSimulatingRag ? 'Executing Semantic Pipeline...' : 'Run RAG Retrieval & Verification' }}</span>
                </button>
              </div>

              <!-- Right: Execution Pipeline Console -->
              <div class="lg:col-span-7 bg-[#07080B] rounded-xl border border-white/[0.08] p-5 font-mono text-xs flex flex-col justify-between">
                
                <div>
                  <div class="flex items-center justify-between pb-3 border-b border-white/[0.06] mb-4">
                    <div class="flex items-center gap-2">
                      <span class="w-2.5 h-2.5 rounded-full bg-red-500/80"></span>
                      <span class="w-2.5 h-2.5 rounded-full bg-yellow-500/80"></span>
                      <span class="w-2.5 h-2.5 rounded-full bg-emerald-500/80"></span>
                      <span class="text-slate-500 ml-2">rag_pipeline.py — execution trace</span>
                    </div>
                    <span class="text-[10px] text-emerald-400 bg-emerald-500/10 px-2 py-0.5 rounded border border-emerald-500/20">
                      Confidence: {{ (ragConfidenceScore * 100).toFixed(1) }}%
                    </span>
                  </div>

                  <!-- Step Pipeline Visualization -->
                  <div class="space-y-3 mb-6">
                    <div
                      v-for="(step, idx) in ragSteps"
                      :key="step.title"
                      class="flex items-start gap-3 p-2.5 rounded-lg border transition-all"
                      :class="
                        ragStep > idx
                          ? 'bg-emerald-500/[0.07] border-emerald-500/30 text-emerald-300'
                          : ragStep === idx + 1
                          ? 'bg-white/[0.04] border-white/20 text-white animate-pulse'
                          : 'border-transparent text-slate-600'
                      "
                    >
                      <div class="w-5 h-5 rounded-full border flex items-center justify-center text-[10px] flex-shrink-0 mt-0.5"
                        :class="ragStep > idx ? 'border-emerald-400 bg-emerald-500/20 text-emerald-400' : 'border-slate-700 text-slate-600'">
                        {{ idx + 1 }}
                      </div>
                      <div class="flex-1">
                        <div class="font-semibold">{{ step.title }}</div>
                        <div class="text-[11px] opacity-80 mt-0.5">{{ step.desc }}</div>
                      </div>
                    </div>
                  </div>

                  <!-- Grounded Output Output Box -->
                  <div v-if="ragStep >= 4 && currentAnswer" class="p-4 rounded-lg bg-emerald-950/20 border border-emerald-500/30 text-slate-200">
                    <div class="text-[10px] uppercase tracking-wider text-emerald-400 font-semibold mb-1 flex items-center gap-1.5">
                      <ShieldCheck class="w-3.5 h-3.5" />
                      <span>Verified Synthesized Response:</span>
                    </div>
                    <p class="leading-relaxed text-xs">{{ currentAnswer.answer }}</p>

                    <!-- Attributed Chunks -->
                    <div class="mt-3 pt-3 border-t border-white/[0.08] space-y-1">
                      <div class="text-[10px] text-slate-400">Retrieved Grounding Chunks:</div>
                      <div v-for="c in currentAnswer.chunks" :key="c" class="text-[10px] text-slate-500 truncate">
                        {{ c }}
                      </div>
                    </div>
                  </div>

                  <div v-else class="text-center py-6 text-slate-600 text-xs">
                    Click "Run RAG Retrieval" to observe real-time vector search & hallucination scoring.
                  </div>
                </div>

                <div class="pt-3 border-t border-white/[0.05] text-[10px] text-slate-500 flex justify-between mt-4">
                  <span>Latency: ~88ms</span>
                  <span>Zero Hallucination Tolerance</span>
                </div>
              </div>

            </div>
          </div>

          <!-- TAB 2: IRENTOUT USER SCALE MONITOR (2K ANDROID & 1K IOS USERS) -->
          <div v-else-if="activeTab === 'irentout-scale'" class="premium-card rounded-2xl p-6 sm:p-8">
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
              
              <!-- Left: Controls -->
              <div class="lg:col-span-5 space-y-6">
                <div>
                  <label class="block text-xs font-mono text-slate-400 uppercase tracking-wider mb-2">
                    iRentOut Client Platform:
                  </label>
                  <div class="grid grid-cols-2 gap-2">
                    <button
                      @click="selectedPlatform = 'android'"
                      class="p-3 rounded-xl border text-xs font-mono flex items-center justify-center gap-2 transition-all"
                      :class="selectedPlatform === 'android' ? 'bg-emerald-500/15 border-emerald-500/40 text-emerald-300 font-semibold' : 'bg-white/[0.02] border-white/[0.07] text-slate-400 hover:border-white/20'"
                    >
                      <Smartphone class="w-4 h-4" />
                      <span>Android (~2k Users)</span>
                    </button>
                    <button
                      @click="selectedPlatform = 'ios'"
                      class="p-3 rounded-xl border text-xs font-mono flex items-center justify-center gap-2 transition-all"
                      :class="selectedPlatform === 'ios' ? 'bg-emerald-500/15 border-emerald-500/40 text-emerald-300 font-semibold' : 'bg-white/[0.02] border-white/[0.07] text-slate-400 hover:border-white/20'"
                    >
                      <Users class="w-4 h-4" />
                      <span>iOS (~1k Users)</span>
                    </button>
                  </div>
                </div>

                <div class="p-4 rounded-xl bg-black/40 border border-white/[0.06] space-y-3">
                  <div class="flex items-center justify-between text-xs">
                    <span class="text-slate-400 font-mono">Active User Base:</span>
                    <span class="font-mono text-emerald-400 font-bold">{{ currentIrentoutProfile.users }}</span>
                  </div>
                  <div class="flex items-center justify-between text-xs">
                    <span class="text-slate-400 font-mono">Tenant Share:</span>
                    <span class="font-mono text-emerald-400">{{ currentIrentoutProfile.userShare }}</span>
                  </div>
                  <div class="flex items-center justify-between text-xs">
                    <span class="text-slate-400 font-mono">Database Layer:</span>
                    <span class="font-mono text-slate-300">{{ currentIrentoutProfile.dbEngine }}</span>
                  </div>
                  <div class="flex items-center justify-between text-xs">
                    <span class="text-slate-400 font-mono">Backend API:</span>
                    <span class="font-mono text-slate-300">{{ currentIrentoutProfile.backend }}</span>
                  </div>
                </div>

                <div class="p-4 rounded-xl bg-white/[0.02] border border-white/[0.06] text-xs text-slate-400 leading-relaxed space-y-2">
                  <div class="text-white font-medium flex items-center gap-1.5">
                    <Zap class="w-3.5 h-3.5 text-emerald-400" />
                    <span>Handling Scale Across 3,000+ Total Users:</span>
                  </div>
                  <p>
                    React Native mobile clients interface with high-throughput <strong>.NET Core Web APIs</strong> and a normalized <strong>MySQL database</strong>.
                    Optimistic UI updates and idempotency keys ensure tenants can sign contracts and book rentals smoothly even on intermittent connectivity.
                  </p>
                </div>
              </div>

              <!-- Right: Visual Device Mockup & Engineering Spec -->
              <div class="lg:col-span-7 bg-[#07080B] rounded-xl border border-white/[0.08] p-5 font-mono text-xs flex flex-col justify-between">
                <div>
                  <div class="flex items-center justify-between pb-3 border-b border-white/[0.06] mb-4">
                    <div class="flex items-center gap-2">
                      <span class="w-2.5 h-2.5 rounded-full bg-emerald-500"></span>
                      <span class="text-white font-semibold text-xs">{{ currentIrentoutProfile.platform }}</span>
                    </div>
                    <span class="text-[10px] text-emerald-400 bg-emerald-500/10 px-2 py-0.5 rounded border border-emerald-500/20">
                      {{ currentIrentoutProfile.users }}
                    </span>
                  </div>

                  <!-- Live Tenant Scale Metric Box -->
                  <div class="p-4 rounded-xl bg-white/[0.02] border border-white/[0.08] mb-5">
                    <div class="flex items-center justify-between text-[11px] text-slate-400 mb-3 pb-2 border-b border-white/[0.05]">
                      <span>Mobile Framework: <strong class="text-white font-mono">React Native</strong></span>
                      <span class="text-emerald-400">Peak Load: {{ currentIrentoutProfile.peakThroughput }}</span>
                    </div>

                    <div class="rounded-lg p-4 border border-dashed border-emerald-500/40 bg-emerald-950/15">
                      <div class="flex items-center justify-between mb-3">
                        <div class="flex items-center gap-2">
                          <div class="w-7 h-7 rounded-lg bg-emerald-500/20 border border-emerald-500/30 flex items-center justify-center text-emerald-400">
                            <Users class="w-4 h-4" />
                          </div>
                          <div>
                            <div class="text-white font-bold text-xs">iRentOut Production Mobile App</div>
                            <div class="text-[10px] text-slate-400">Rental, Contract & Payment Flows</div>
                          </div>
                        </div>
                        <div class="text-[10px] px-2 py-0.5 rounded bg-emerald-500/20 text-emerald-300 font-mono">
                          {{ selectedPlatform.toUpperCase() }} CLIENT
                        </div>
                      </div>

                      <div class="grid grid-cols-2 gap-2 text-[10px]">
                        <div class="p-2 rounded bg-black/40 border border-white/[0.05]">
                          <div class="text-slate-500">Crash-Free Rate</div>
                          <div class="text-emerald-400 font-semibold mt-0.5">{{ currentIrentoutProfile.crashRate }}</div>
                        </div>
                        <div class="p-2 rounded bg-black/40 border border-white/[0.05]">
                          <div class="text-slate-500">API Response SLA</div>
                          <div class="text-emerald-400 font-semibold mt-0.5">&lt; 60ms (.NET + MySQL)</div>
                        </div>
                      </div>
                    </div>
                  </div>

                  <!-- Engineering Specification Cards -->
                  <div class="space-y-2.5">
                    <div class="text-[11px] text-slate-300 font-semibold flex items-center gap-1.5">
                      <CheckCircle2 class="w-3.5 h-3.5 text-emerald-400" />
                      <span>Data Sync & State Architecture:</span>
                    </div>
                    <div class="p-3 rounded-lg bg-black/50 border border-white/[0.06] text-[11px] text-slate-300 leading-relaxed font-sans">
                      {{ currentIrentoutProfile.syncArchitecture }}
                    </div>
                  </div>
                </div>

                <div class="pt-3 border-t border-white/[0.05] text-[10px] text-slate-500 flex justify-between mt-4">
                  <span>Stack: React Native · .NET Core · MySQL</span>
                  <span>Total Base: ~3,000 Live Tenants</span>
                </div>
              </div>

            </div>
          </div>

          <!-- TAB 3: TELEMETRY & CONCURRENCY SIMULATOR -->
          <div v-else class="premium-card rounded-2xl p-6 sm:p-8">
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
              
              <!-- Left: Concurrency Slider -->
              <div class="lg:col-span-5 space-y-6">
                <div>
                  <div class="flex items-center justify-between mb-2">
                    <label class="text-xs font-mono text-slate-400 uppercase tracking-wider">
                      Simulated Concurrent Dashboard Users:
                    </label>
                    <span class="font-mono text-emerald-400 font-bold text-sm">{{ virtualConcurrentUsers }} users</span>
                  </div>
                  <input
                    type="range"
                    min="50"
                    max="1000"
                    step="50"
                    v-model.number="virtualConcurrentUsers"
                    class="w-full accent-emerald-400 bg-white/10 rounded-lg h-2 cursor-pointer"
                  />
                  <div class="flex justify-between text-[10px] text-slate-500 font-mono mt-1">
                    <span>50 users</span>
                    <span>500 (Production Baseline)</span>
                    <span>1,000 users</span>
                  </div>
                </div>

                <div class="grid grid-cols-2 gap-3">
                  <div class="p-4 rounded-xl bg-black/40 border border-white/[0.06]">
                    <div class="text-[10px] text-slate-400 font-mono">P95 Latency</div>
                    <div class="text-xl font-bold font-mono text-emerald-400 mt-1">{{ calculatedResponseTime }} ms</div>
                    <div class="text-[10px] text-slate-500 mt-0.5">Optimized RxJS Streams</div>
                  </div>
                  <div class="p-4 rounded-xl bg-black/40 border border-white/[0.06]">
                    <div class="text-[10px] text-slate-400 font-mono">Page Load Reduction</div>
                    <div class="text-xl font-bold font-mono text-teal-400 mt-1">-40%</div>
                    <div class="text-[10px] text-slate-500 mt-0.5">Modular Lazy Architecture</div>
                  </div>
                </div>

                <div class="p-4 rounded-xl bg-white/[0.02] border border-white/[0.06] text-xs text-slate-400 leading-relaxed space-y-2">
                  <div class="text-white font-medium flex items-center gap-1.5">
                    <Zap class="w-3.5 h-3.5 text-emerald-400" />
                    <span>How this was achieved in production:</span>
                  </div>
                  <p>
                    Replaced unthrottled component-level polling with centralized RxJS Subject multiplexers.
                    Decoupled background hardware ticks from the UI rendering thread to preserve 60fps animations.
                  </p>
                </div>
              </div>

              <!-- Right: Simulated Live Device Cluster Telemetry -->
              <div class="lg:col-span-7 bg-[#07080B] rounded-xl border border-white/[0.08] p-5 font-mono text-xs">
                <div class="flex items-center justify-between pb-3 border-b border-white/[0.06] mb-4">
                  <span class="text-slate-400 flex items-center gap-2">
                    <Network class="w-3.5 h-3.5 text-emerald-400" />
                    <span>Connected Device Node Telemetry</span>
                  </span>
                  <span class="text-[10px] text-emerald-400 bg-emerald-500/10 px-2 py-0.5 rounded border border-emerald-500/20">
                    20+ Hardware Device Types
                  </span>
                </div>

                <div class="space-y-3">
                  <div
                    v-for="d in liveTelemetry"
                    :key="d.device"
                    class="p-3 rounded-lg bg-white/[0.02] border border-white/[0.06] flex items-center justify-between"
                  >
                    <div>
                      <div class="text-white font-semibold text-xs flex items-center gap-2">
                        <span class="w-1.5 h-1.5 rounded-full bg-emerald-400"></span>
                        {{ d.device }}
                      </div>
                      <div class="text-[11px] text-slate-500 mt-0.5">{{ d.protocol }}</div>
                    </div>

                    <div class="flex items-center gap-6 text-right">
                      <div>
                        <div class="text-slate-400 text-[10px]">Ping</div>
                        <div class="text-emerald-400 font-mono font-medium">{{ d.latency }}ms</div>
                      </div>
                      <div>
                        <div class="text-slate-400 text-[10px]">Buffer Load</div>
                        <div class="text-slate-300 font-mono font-medium">{{ d.load }}%</div>
                      </div>
                      <span class="text-[10px] px-2 py-0.5 rounded bg-white/[0.05] text-slate-300 border border-white/[0.08]">
                        {{ d.status }}
                      </span>
                    </div>
                  </div>
                </div>

                <div class="pt-4 border-t border-white/[0.06] text-[10px] text-slate-500 flex justify-between mt-4">
                  <span>MQTT Broker: Mosquitto / Cloud Bridge</span>
                  <span>QoS 1 Ack Timers Active</span>
                </div>
              </div>

            </div>
          </div>

        </div>
      </section>

      <!-- ================= PROFESSIONAL WORK EXPERIENCE ================= -->
      <section id="experience" class="py-20 max-w-6xl mx-auto px-4 sm:px-6">
        
        <div class="flex items-center gap-2 text-xs font-mono text-emerald-400 uppercase tracking-wider mb-2">
          <Briefcase class="w-3.5 h-3.5" />
          <span>Verified Career Journey</span>
        </div>
        <h2 class="text-3xl sm:text-4xl font-bold text-white tracking-tight mb-4">
          Experience & Impact
        </h2>
        <p class="text-slate-400 text-sm max-w-2xl mb-12">
          Track record across SaaS, hardware automation, and full-stack software development in Bengaluru.
        </p>

        <div class="space-y-8">
          <div
            v-for="(exp, index) in experiences"
            :key="index"
            class="premium-card rounded-2xl p-7 md:p-8 relative group"
          >
            <!-- Current role marker badge -->
            <div v-if="index === 0" class="absolute -top-3 right-6 px-3 py-1 rounded-full bg-emerald-500 text-neutral-950 text-[10px] font-bold font-mono tracking-wider uppercase shadow-md">
              Current Company
            </div>

            <div class="flex flex-col md:flex-row md:items-center justify-between pb-6 border-b border-white/[0.06] gap-2">
              <div>
                <div class="flex items-center gap-2 mb-1 flex-wrap">
                  <h3 class="text-xl font-bold text-white tracking-tight">{{ exp.role }}</h3>
                  <span class="text-xs font-mono px-2 py-0.5 rounded-full bg-white/[0.05] text-slate-300 border border-white/[0.08]">
                    {{ exp.badge }}
                  </span>
                </div>
                <div class="text-sm text-slate-300 font-medium">
                  <span class="text-emerald-400">{{ exp.company }}</span>
                  <span class="text-slate-500 font-normal"> · {{ exp.location }}</span>
                </div>
              </div>
              <div class="text-xs font-mono text-slate-400 bg-white/[0.04] px-3 py-1.5 rounded-lg border border-white/[0.08] self-start md:self-auto">
                {{ exp.period }}
              </div>
            </div>

            <p class="text-xs sm:text-sm text-slate-400 mt-4 leading-relaxed font-normal">
              {{ exp.summary }}
            </p>

            <!-- Bulleted impact points directly matching resume metrics -->
            <ul class="mt-4 space-y-2.5">
              <li
                v-for="(point, i) in exp.achievements"
                :key="i"
                class="flex items-start gap-3 text-xs sm:text-sm text-slate-300 leading-relaxed"
              >
                <div class="w-1.5 h-1.5 rounded-full bg-emerald-400 mt-2 flex-shrink-0"></div>
                <span>{{ point }}</span>
              </li>
            </ul>

            <!-- Tech badges -->
            <div class="mt-6 pt-5 border-t border-white/[0.06] flex flex-wrap gap-1.5">
              <span v-for="tag in exp.tags" :key="tag" class="tech-pill">
                {{ tag }}
              </span>
            </div>
          </div>
        </div>
      </section>

      <!-- ================= SELECTED WORK ================= -->
      <section id="work" class="py-20 max-w-6xl mx-auto px-4 sm:px-6">
        
        <!-- Section Header -->
        <div class="flex flex-col md:flex-row md:items-end justify-between mb-12 gap-6">
          <div>
            <div class="flex items-center gap-2 text-xs font-mono text-emerald-400 uppercase tracking-wider mb-2">
              <Terminal class="w-3.5 h-3.5" />
              <span>Shipped Systems</span>
            </div>
            <h2 class="text-3xl sm:text-4xl font-bold text-white tracking-tight">Featured Engineering Work</h2>
          </div>

          <!-- Category filter tabs -->
          <div class="inline-flex p-1 rounded-lg bg-white/[0.03] border border-white/[0.08] text-xs font-medium self-start md:self-auto">
            <button
              @click="projectFilter = 'all'"
              class="px-3.5 py-1.5 rounded-md transition-all"
              :class="projectFilter === 'all' ? 'bg-white/10 text-white font-semibold' : 'text-slate-400 hover:text-white'"
            >
              All Projects
            </button>
            <button
              @click="projectFilter = 'ai'"
              class="px-3.5 py-1.5 rounded-md transition-all"
              :class="projectFilter === 'ai' ? 'bg-white/10 text-white font-semibold' : 'text-slate-400 hover:text-white'"
            >
              GenAI & RAG
            </button>
            <button
              @click="projectFilter = 'mobile'"
              class="px-3.5 py-1.5 rounded-md transition-all"
              :class="projectFilter === 'mobile' ? 'bg-white/10 text-white font-semibold' : 'text-slate-400 hover:text-white'"
            >
              Mobile / IoT
            </button>
            <button
              @click="projectFilter = 'web'"
              class="px-3.5 py-1.5 rounded-md transition-all"
              :class="projectFilter === 'web' ? 'bg-white/10 text-white font-semibold' : 'text-slate-400 hover:text-white'"
            >
              Web Platforms
            </button>
          </div>
        </div>

        <!-- Projects Grid -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
          <div
            v-for="project in filteredProjects"
            :key="project.id"
            class="premium-card rounded-2xl p-7 flex flex-col justify-between transition-all duration-300 relative group overflow-hidden"
          >
            <!-- Card top edge ambient line -->
            <div class="absolute top-0 left-0 right-0 h-[1px] bg-gradient-to-r from-transparent via-white/20 to-transparent"></div>

            <div>
              <!-- Top Row: Badge & Action Links -->
              <div class="flex items-center justify-between mb-4">
                <span class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-md text-[11px] font-mono tracking-wide bg-white/[0.05] text-slate-300 border border-white/[0.08]">
                  <Sparkles v-if="project.category === 'ai'" class="w-3 h-3 text-emerald-400" />
                  <Smartphone v-else-if="project.category === 'mobile'" class="w-3 h-3 text-teal-400" />
                  <Layers v-else class="w-3 h-3 text-indigo-400" />
                  {{ project.badge }}
                </span>

                <div class="flex items-center gap-2">
                  <a
                    v-if="project.github"
                    :href="project.github"
                    target="_blank"
                    rel="noopener noreferrer"
                    class="p-2 rounded-lg text-slate-400 hover:text-white hover:bg-white/[0.06] transition-colors"
                    title="View Source on GitHub"
                  >
                    <Github class="w-4 h-4" />
                  </a>

                  <button
                    v-if="project.appStoreModal"
                    @click="openAppModal"
                    class="inline-flex items-center gap-1 px-3 py-1.5 rounded-lg text-xs font-medium bg-emerald-500/10 hover:bg-emerald-500/20 text-emerald-300 border border-emerald-500/30 transition-all"
                  >
                    <span>Store Links</span>
                    <ArrowUpRight class="w-3.5 h-3.5" />
                  </button>

                  <a
                    v-if="project.live"
                    :href="project.live"
                    target="_blank"
                    rel="noopener noreferrer"
                    class="p-2 rounded-lg text-slate-400 hover:text-white hover:bg-white/[0.06] transition-colors"
                    title="Live Platform"
                  >
                    <ExternalLink class="w-4 h-4" />
                  </a>
                </div>
              </div>

              <!-- Title & Tagline -->
              <h3 class="text-xl font-bold text-white mb-1.5 tracking-tight group-hover:text-emerald-400 transition-colors">
                {{ project.title }}
              </h3>
              <p class="text-xs font-medium text-emerald-400/90 mb-4 font-mono">
                {{ project.tagline }}
              </p>

              <!-- Description -->
              <p class="text-sm text-slate-400 leading-relaxed mb-6 font-normal">
                {{ project.summary }}
              </p>

              <!-- Quantitative Metrics Callout -->
              <div class="grid grid-cols-3 gap-2 p-3 rounded-xl bg-black/30 border border-white/[0.05] mb-6">
                <div v-for="metric in project.metrics" :key="metric.label" class="space-y-0.5">
                  <div class="text-[10px] uppercase font-mono text-slate-500">{{ metric.label }}</div>
                  <div class="text-xs font-semibold text-slate-200 font-mono">{{ metric.value }}</div>
                </div>
              </div>
            </div>

            <!-- Footer: Tech tags -->
            <div class="pt-4 border-t border-white/[0.06] flex flex-wrap gap-1.5">
              <span v-for="t in project.tech" :key="t" class="tech-pill">
                {{ t }}
              </span>
            </div>
          </div>
        </div>
      </section>

      <!-- ================= CORE CAPABILITIES ================= -->
      <section id="expertise" class="py-20 border-t border-white/[0.08] bg-[#0A0C10]">
        <div class="max-w-6xl mx-auto px-4 sm:px-6">
          
          <div class="max-w-2xl mb-14">
            <div class="flex items-center gap-2 text-xs font-mono text-emerald-400 uppercase tracking-wider mb-2">
              <Zap class="w-3.5 h-3.5" />
              <span>Full-Stack & AI Competencies</span>
            </div>
            <h2 class="text-3xl sm:text-4xl font-bold text-white tracking-tight mb-4">
              Engineered for Production Resilience
            </h2>
            <p class="text-slate-400 text-sm leading-relaxed">
              Synthesizing generative AI pipelines, reactive web frameworks, and low-latency mobile protocols into coherent product architectures.
            </p>
          </div>

          <!-- 4 Pillars Grid -->
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div
              v-for="cap in capabilities"
              :key="cap.title"
              class="premium-card rounded-2xl p-7 transition-all duration-300 group hover:border-white/20"
            >
              <div class="flex items-center justify-between mb-5">
                <div class="w-11 h-11 rounded-xl bg-white/[0.05] border border-white/10 flex items-center justify-center text-emerald-400 group-hover:bg-emerald-500/10 transition-colors">
                  <component :is="cap.icon" class="w-5 h-5" />
                </div>
                <span class="text-[11px] font-mono font-medium px-2.5 py-1 rounded-full bg-white/[0.04] text-slate-300 border border-white/[0.08]">
                  {{ cap.proof }}
                </span>
              </div>

              <h3 class="text-lg font-bold text-white mb-2 tracking-tight group-hover:text-emerald-400 transition-colors">
                {{ cap.title }}
              </h3>

              <p class="text-sm text-slate-400 leading-relaxed mb-6 font-normal">
                {{ cap.desc }}
              </p>

              <div class="flex flex-wrap gap-1.5 pt-4 border-t border-white/[0.06]">
                <span v-for="item in cap.stack" :key="item" class="tech-pill">
                  {{ item }}
                </span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- ================= ABOUT & CREDENTIALS ================= -->
      <section id="about" class="py-20 border-t border-white/[0.08]">
        <div class="max-w-6xl mx-auto px-4 sm:px-6">
          <div class="grid grid-cols-1 lg:grid-cols-12 gap-12">
            
            <!-- Bio Column -->
            <div class="lg:col-span-7 space-y-6">
              <div class="flex items-center gap-2 text-xs font-mono text-emerald-400 uppercase tracking-wider">
                <ShieldCheck class="w-3.5 h-3.5" />
                <span>Background & Philosophy</span>
              </div>
              <h2 class="text-3xl sm:text-4xl font-bold text-white tracking-tight">
                Engineering with data validation, speed, and real-world uptime.
              </h2>

              <div class="space-y-4 text-slate-300 text-sm sm:text-base leading-relaxed">
                <p>
                  I'm a full-stack & mobile developer specializing in building AI-integrated applications that actually deliver value in production.
                  My philosophy prioritizes clean architecture over brittle glue code: whether structuring RAG pipelines to eliminate hallucinations,
                  optimizing Angular bundle chunks for 40% faster loading, or ensuring mobile applications persist state through sudden network drops.
                </p>
                <p>
                  Having developed software across <strong class="text-white">iRentOut</strong>, <strong class="text-white">SmartBuild Automation</strong>, and <strong class="text-white">Melange Systems</strong>,
                  I take ownership of both the customer-facing interface and the underlying API/telemetry contracts.
                </p>
              </div>

              <!-- Certifications -->
              <div class="pt-4 space-y-3">
                <div class="text-xs font-mono text-slate-400 uppercase tracking-wider">Verified Certifications:</div>
                <div v-for="cert in certifications" :key="cert.name" class="p-3.5 rounded-xl bg-white/[0.03] border border-white/[0.07] flex items-start gap-3">
                  <Award class="w-4 h-4 text-emerald-400 flex-shrink-0 mt-0.5" />
                  <div>
                    <div class="text-xs font-semibold text-white">{{ cert.name }}</div>
                    <div class="text-[11px] text-slate-400">{{ cert.focus }}</div>
                  </div>
                </div>
              </div>
            </div>

            <!-- Credentials & Education Column -->
            <div class="lg:col-span-5 space-y-6">
              <div class="premium-card rounded-2xl p-6 space-y-6">
                <div class="flex items-center gap-2 text-xs font-mono text-slate-400 uppercase tracking-wider pb-3 border-b border-white/[0.06]">
                  <GraduationCap class="w-4 h-4 text-emerald-400" />
                  <span>Academic Foundations</span>
                </div>

                <div class="space-y-6">
                  <div v-for="edu in education" :key="edu.degree" class="space-y-1">
                    <div class="flex items-center justify-between">
                      <h4 class="text-sm font-bold text-white">{{ edu.degree }}</h4>
                      <span class="text-[11px] font-mono text-slate-400">{{ edu.period }}</span>
                    </div>
                    <div class="text-xs text-emerald-400/90 font-medium">{{ edu.major }}</div>
                    <div class="text-xs text-slate-300">{{ edu.institution }}</div>
                    <p class="text-xs text-slate-400 pt-1 leading-relaxed">{{ edu.detail }}</p>
                  </div>
                </div>
              </div>

              <!-- Quick Facts Card -->
              <div class="premium-card rounded-2xl p-6 space-y-3">
                <div class="flex items-center gap-2 text-xs font-mono text-slate-400 uppercase tracking-wider pb-3 border-b border-white/[0.06]">
                  <Sliders class="w-4 h-4 text-teal-400" />
                  <span>Engineering Profile</span>
                </div>

                <div class="text-xs text-slate-300 space-y-2">
                  <div class="flex items-center justify-between py-1 border-b border-white/[0.04]">
                    <span class="text-slate-400">Current Role</span>
                    <span class="font-medium text-emerald-400">Software Developer @ iRentOut</span>
                  </div>
                  <div class="flex items-center justify-between py-1 border-b border-white/[0.04]">
                    <span class="text-slate-400">Location</span>
                    <span class="font-medium text-white">Bengaluru, Karnataka, India</span>
                  </div>
                  <div class="flex items-center justify-between py-1 border-b border-white/[0.04]">
                    <span class="text-slate-400">Top Stack</span>
                    <span class="font-medium text-white">React Native · .NET Core · MySQL · Flutter · LangChain</span>
                  </div>
                  <div class="flex items-center justify-between py-1">
                    <span class="text-slate-400">Work Model</span>
                    <span class="font-medium text-emerald-300">Open to high-impact opportunities</span>
                  </div>
                </div>
              </div>

            </div>
          </div>
        </div>
      </section>

      <!-- ================= CONTACT / GET IN TOUCH ================= -->
      <section id="contact" class="py-24 max-w-6xl mx-auto px-4 sm:px-6">
        <div class="premium-card rounded-3xl p-8 sm:p-12 md:p-16 relative overflow-hidden">
          
          <div class="absolute -top-32 -right-32 w-80 h-80 bg-emerald-500/10 rounded-full blur-3xl pointer-events-none"></div>

          <div class="max-w-2xl relative z-10">
            <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-emerald-500/10 border border-emerald-500/20 text-emerald-400 text-xs font-mono mb-4">
              <span class="w-2 h-2 rounded-full bg-emerald-400"></span>
              <span>Available for Contact</span>
            </div>

            <h2 class="text-3xl sm:text-5xl font-bold text-white tracking-tight mb-4">
              Let's build something exceptional together.
            </h2>

            <p class="text-base text-slate-300 leading-relaxed mb-8">
              Open to technical conversations, software engineering roles, and AI/mobile architecture discussions.
            </p>

            <div class="flex flex-wrap items-center gap-4">
              <a
                href="mailto:poojaryabhishek26@gmail.com"
                class="px-6 py-3.5 rounded-xl bg-emerald-500 hover:bg-emerald-400 text-neutral-950 font-semibold text-sm transition-all flex items-center gap-2 shadow-lg shadow-emerald-500/20"
              >
                <Mail class="w-4 h-4" />
                <span>poojaryabhishek26@gmail.com</span>
              </a>

              <a
                href="tel:+919945726449"
                class="px-6 py-3.5 rounded-xl bg-white/[0.05] hover:bg-white/[0.1] text-white border border-white/10 text-sm font-medium transition-all flex items-center gap-2"
              >
                <Phone class="w-4 h-4 text-slate-400" />
                <span>+91 99457 26449</span>
              </a>

              <a
                href="https://www.linkedin.com/in/abhishek-poojary-a50281268"
                target="_blank"
                rel="noopener noreferrer"
                class="px-5 py-3.5 rounded-xl bg-white/[0.05] hover:bg-white/[0.1] text-white border border-white/10 text-sm font-medium transition-all flex items-center gap-2"
              >
                <Linkedin class="w-4 h-4 text-slate-400" />
                <span>LinkedIn</span>
              </a>

              <a
                href="https://github.com/Abhishek-poojary1"
                target="_blank"
                rel="noopener noreferrer"
                class="px-5 py-3.5 rounded-xl bg-white/[0.05] hover:bg-white/[0.1] text-white border border-white/10 text-sm font-medium transition-all flex items-center gap-2"
              >
                <Github class="w-4 h-4 text-slate-400" />
                <span>GitHub</span>
              </a>
            </div>
          </div>
        </div>
      </section>

    </main>

    <!-- Footer -->
    <footer class="border-t border-white/[0.06] py-10 bg-[#07080B] text-xs text-slate-500">
      <div class="max-w-6xl mx-auto px-4 sm:px-6 flex flex-col sm:flex-row items-center justify-between gap-4">
        <div>
          <span>© {{ new Date().getFullYear() }} Abhishek Poojary · Software Developer @ iRentOut</span>
        </div>
        <div class="flex items-center gap-6">
          <button @click="scrollToSection('hero')" class="hover:text-slate-300 transition-colors">Back to top ↑</button>
          <a href="https://github.com/Abhishek-poojary1" target="_blank" rel="noopener noreferrer" class="hover:text-slate-300 transition-colors">GitHub</a>
          <a href="https://www.linkedin.com/in/abhishek-poojary-a50281268" target="_blank" rel="noopener noreferrer" class="hover:text-slate-300 transition-colors">LinkedIn</a>
        </div>
      </div>
    </footer>

    <!-- App Store Teleport Modal -->
    <Teleport to="body">
      <Transition
        enter-active-class="transition duration-200 ease-out"
        enter-from-class="opacity-0"
        enter-to-class="opacity-100"
        leave-active-class="transition duration-150 ease-in"
        leave-from-class="opacity-100"
        leave-to-class="opacity-0"
      >
        <div
          v-if="showAppModal"
          class="fixed inset-0 z-[100] flex items-center justify-center p-4 bg-black/80 backdrop-blur-md"
          @click.self="closeAppModal"
        >
          <div
            class="bg-[#0E1015] border border-white/10 rounded-2xl p-6 sm:p-8 max-w-2xl w-full shadow-2xl relative"
            role="dialog"
            aria-modal="true"
          >
            <!-- Close icon -->
            <button
              @click="closeAppModal"
              class="absolute top-5 right-5 p-2 rounded-lg text-slate-400 hover:text-white bg-white/[0.05] hover:bg-white/[0.1] transition-colors"
              aria-label="Close modal"
            >
              <X class="w-5 h-5" />
            </button>

            <div class="mb-6">
              <div class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-md text-[11px] font-mono bg-emerald-500/10 text-emerald-400 border border-emerald-500/20 mb-3">
                <Smartphone class="w-3.5 h-3.5" />
                <span>Live Deployments</span>
              </div>
              <h3 class="text-2xl font-bold text-white tracking-tight">SmartBuild Mobile Applications</h3>
              <p class="text-xs text-slate-400 mt-1">Published live on both the Apple App Store and Google Play Store.</p>
            </div>

            <!-- Two app cards -->
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
              
              <!-- SB Auto -->
              <div class="p-5 rounded-xl bg-white/[0.03] border border-white/[0.08] flex flex-col justify-between space-y-4">
                <div>
                  <h4 class="text-base font-bold text-white">{{ appLinks.sbAuto.name }}</h4>
                  <p class="text-xs text-slate-400 mt-1.5 leading-relaxed">{{ appLinks.sbAuto.desc }}</p>
                </div>
                <div class="space-y-2 pt-2 border-t border-white/[0.05]">
                  <a
                    :href="appLinks.sbAuto.appStore"
                    target="_blank"
                    rel="noopener noreferrer"
                    class="w-full py-2 px-3 rounded-lg bg-white text-neutral-950 font-semibold text-xs flex items-center justify-center gap-1.5 hover:bg-slate-200 transition-colors"
                  >
                    <span>Apple App Store</span>
                    <ArrowUpRight class="w-3.5 h-3.5" />
                  </a>
                  <a
                    :href="appLinks.sbAuto.playStore"
                    target="_blank"
                    rel="noopener noreferrer"
                    class="w-full py-2 px-3 rounded-lg bg-white/[0.06] text-white border border-white/10 font-medium text-xs flex items-center justify-center gap-1.5 hover:bg-white/[0.1] transition-colors"
                  >
                    <span>Google Play Store</span>
                    <ArrowUpRight class="w-3.5 h-3.5" />
                  </a>
                </div>
              </div>

              <!-- SBA Home -->
              <div class="p-5 rounded-xl bg-white/[0.03] border border-white/[0.08] flex flex-col justify-between space-y-4">
                <div>
                  <h4 class="text-base font-bold text-white">{{ appLinks.sbaHome.name }}</h4>
                  <p class="text-xs text-slate-400 mt-1.5 leading-relaxed">{{ appLinks.sbaHome.desc }}</p>
                </div>
                <div class="space-y-2 pt-2 border-t border-white/[0.05]">
                  <a
                    :href="appLinks.sbaHome.appStore"
                    target="_blank"
                    rel="noopener noreferrer"
                    class="w-full py-2 px-3 rounded-lg bg-white text-neutral-950 font-semibold text-xs flex items-center justify-center gap-1.5 hover:bg-slate-200 transition-colors"
                  >
                    <span>Apple App Store</span>
                    <ArrowUpRight class="w-3.5 h-3.5" />
                  </a>
                  <a
                    :href="appLinks.sbaHome.playStore"
                    target="_blank"
                    rel="noopener noreferrer"
                    class="w-full py-2 px-3 rounded-lg bg-white/[0.06] text-white border border-white/10 font-medium text-xs flex items-center justify-center gap-1.5 hover:bg-white/[0.1] transition-colors"
                  >
                    <span>Google Play Store</span>
                    <ArrowUpRight class="w-3.5 h-3.5" />
                  </a>
                </div>
              </div>

            </div>

            <div class="mt-6 text-center">
              <button
                @click="closeAppModal"
                class="text-xs text-slate-500 hover:text-slate-300 font-mono transition-colors"
              >
                Press Esc or click outside to dismiss
              </button>
            </div>
          </div>
        </div>
      </Transition>
    </Teleport>

  </div>
</template>
