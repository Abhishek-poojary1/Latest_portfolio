<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
import {
  Menu,
  X,
  ArrowRight,
  Github,
  Linkedin,
  Brain,
  Smartphone,
  Cpu,
  Globe,
  Server,
  Wifi,
  Cloud,
  Mail,
  Phone,
  GraduationCap,
  Award,
  Trophy,
  Layout,
  Bot,
  Home,
  ShoppingBag,
} from "lucide-vue-next";

// Navigation
const navItems = [
  { id: "about", label: "About" },
  { id: "skills", label: "Skills" },
  { id: "experience", label: "Experience" },
  { id: "projects", label: "Projects" },
];

const mobileMenuOpen = ref(false);
const scrolled = ref(false);

// Typewriter effect
const roles = [
  "Generative AI Engineer",
  "Flutter Developer",
  "Full Stack Developer",
  "Software Developer",
];
const currentRoleIndex = ref(0);
const currentText = ref("");
const isDeleting = ref(false);

// Intersection Observer for scroll animations
const heroVisible = ref(false);
const aboutVisible = ref(false);
const skillsVisible = ref(false);
const expVisible = ref(false);
const projectsVisible = ref(false);
const contactVisible = ref(false);

// Stats
const stats = [
  { value: "2+", label: "Years Exp" },
  { value: "3+", label: "AI Models" },
  { value: "40%", label: "Performance Gain" },
];

// About tags
const aboutTags = [
  "Problem Solver",
  "AI Enthusiast",
  "Software Craftsman",
  "Mobile First",
];

// Expertise areas
const expertiseAreas = [
  { icon: Brain, title: "GenAI", desc: "LLMs & RAG", color: "text-purple-400" },
  {
    icon: Smartphone,
    title: "Flutter",
    desc: "iOS & Android",
    color: "text-blue-400",
  },
  {
    icon: Cpu,
    title: "IoT",
    desc: "MQTT & Real-time",
    color: "text-green-400",
  },
  {
    icon: Globe,
    title: "Web",
    desc: "React & Angular",
    color: "text-pink-400",
  },
];

// Skill categories
const skillCategories = [
  {
    title: "Generative AI",
    icon: Brain,
    bgColor: "bg-purple-500/20",
    iconColor: "text-purple-400",
    skills: [
      "OpenAI API",
      "LangChain",
      "RAG",
      "FAISS",
      "Embeddings",
      "Prompt Engineering",
      "Vector DBs",
    ],
  },
  {
    title: "Mobile Dev",
    icon: Smartphone,
    bgColor: "bg-blue-500/20",
    iconColor: "text-blue-400",
    skills: [
      "Flutter",
      "Dart",
      "Provider",
      "Bloc",
      "Riverpod",
      "SQLite",
      "Firebase",
      "Clean Architecture",
    ],
  },
  {
    title: "Web Frontend",
    icon: Layout,
    bgColor: "bg-pink-500/20",
    iconColor: "text-pink-400",
    skills: [
      "React.js",
      "Angular",
      "TypeScript",
      "Vite",
      "HTML5/CSS3",
      "Responsive Design",
      "RxJS",
    ],
  },
  {
    title: "Backend",
    icon: Server,
    bgColor: "bg-green-500/20",
    iconColor: "text-green-400",
    skills: [
      "Python",
      "FastAPI",
      "Node.js",
      "Express.js",
      "REST APIs",
      "MongoDB",
      "Firestore",
    ],
  },
  {
    title: "IoT & Real-Time",
    icon: Wifi,
    bgColor: "bg-yellow-500/20",
    iconColor: "text-yellow-400",
    skills: [
      "MQTT",
      "WebSockets",
      "Device Communication",
      "Automation",
      "Real-time Sync",
    ],
  },
  {
    title: "Cloud & Tools",
    icon: Cloud,
    bgColor: "bg-cyan-500/20",
    iconColor: "text-cyan-400",
    skills: ["Docker", "CI/CD", "Git", "Postman", "AWS/GCP", "Vercel", "Figma"],
  },
];

// Experience
const experiences = [
  {
    period: "Jan 2024 - Present",
    role: "Assistant Software Developer",
    company: "SmartBuild Automation Pvt Ltd, Bengaluru",
    description: [
      "Developed production Flutter apps with 40% performance improvement",
      "Integrated  AI features into Flutter Apps",
      "Architected real-time systems using WebSockets & MQTT",
      "Managed 20+ IoT device integrations for App",
    ],
    tech: ["Flutter", "AI Integration", "IoT", "Firebase"],
  },
  {
    period: "Oct 2023 - Jan 2024",
    role: "Software Developer Intern",
    company: "Melange Systems Pvt Ltd",
    description: [
      "Built React.js & Angular frontend modules",
      "Developed Node.js/Express backend APIs",
      "Created reusable UI components improving dev speed",
      "Participated in deployment & documentation",
    ],
    tech: ["React", "Angular", "Node.js", "MongoDB"],
  },
];

// Projects
const projects = [
  {
    title: "AI Smart Support System",
    description:
      "RAG-based document Q&A system using LangChain and OpenAI. Features FAISS vector search, context-aware responses, and REST API integration for mobile/web clients.",
    tech: ["Python", "FastAPI", "LangChain", "FAISS"],
    gradient: "from-purple-600 to-blue-600",
    tag: "Generative AI",
    icon: Bot,
    link: "https://github.com/Abhishek-poojary1/AI_Smart_Support.git",
  },
  {
    title: "SB Auto & SBA Home",
    description:
      "Production-grade IoT automation apps controlling 20+ device types. Features real-time MQTT/WebSocket communication, offline-first SQLite architecture, and cloud sync.",
    tech: ["Flutter", "Firebase", "MQTT", "IoT"],
    gradient: "from-blue-600 to-cyan-600",
    tag: "Live on App Store",
    icon: Home,
    link: "#",
  },
  {
    title: "IoT Control Dashboard",
    description:
      "Angular-based web dashboard for monitoring and controlling IoT devices. Features real-time data visualization, device grouping, and automation rule configuration.",
    tech: ["Angular", "TypeScript", "RxJS", "WebSockets"],
    gradient: "from-pink-600 to-orange-600",
    tag: "Web Platform",
    icon: Layout,
    link: "#",
  },
  {
    title: "E-Commerce Platform",
    description:
      "Cross-platform e-commerce solution with Flutter mobile app and React web frontend. Features real-time inventory, cart management, and secure authentication.",
    tech: ["Flutter", "React", "Firebase", "REST API"],
    gradient: "from-green-600 to-teal-600",
    tag: "Full Stack",
    icon: ShoppingBag,
    link: "#",
  },
];

// Education
const education = [
  {
    degree: "Bachelor of Computer Applications (BCA)",
    school: "B.B. Hegde First Grade College",
    year: "2019 - 2023",
  },
  {
    degree: "Pre-University College (PUC)",
    school: "Govt PU College Uppunda",
    year: "2018 - 2019",
  },
];

// Achievements
const achievements = [
  "40% app performance improvement via optimization",
  "Published 2 live apps on App Store with active users",
  "Implemented 5+ IoT integrations with real-time comms with app",
];

// Social links
const socials = [
  { name: "GitHub", url: "https://github.com/Abhishek-poojary1", icon: Github },
  {
    name: "LinkedIn",
    url: "https://www.linkedin.com/in/abhishek-poojary-a50281268",
    icon: Linkedin,
  },
];
// Modal state
const showAppModal = ref(false);
const modalRef = ref<HTMLElement | null>(null);
// Prevents TS error - used for modal DOM reference
void modalRef;
// App store links (update these with your actual links)
const appLinks = {
  sbAuto: {
    appStore: "https://apps.apple.com/in/app/sb-auto/id6478853565", // Replace with real link
    playStore:
      "https://play.google.com/store/apps/details?id=com.smartbuildauto.SBAuto", // Replace with real link
  },
  sbaHome: {
    appStore: "https://apps.apple.com/in/app/sba-home/id6739250335", // Replace with real link
    playStore:
      "https://play.google.com/store/apps/details?id=com.smartbuildauto.SBAHome", // Replace with real link
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

// Close on escape key
onMounted(() => {
  window.addEventListener("keydown", (e) => {
    if (e.key === "Escape" && showAppModal.value) {
      closeAppModal();
    }
  });
});
// Typewriter logic
// Typewriter logic
const typeWriter = () => {
  const currentRole = roles[currentRoleIndex.value];

  // TypeScript safety check
  if (!currentRole) {
    currentRoleIndex.value = 0;
    setTimeout(typeWriter, 500);
    return;
  }

  if (isDeleting.value) {
    currentText.value = currentRole.substring(0, currentText.value.length - 1);
  } else {
    currentText.value = currentRole.substring(0, currentText.value.length + 1);
  }

  let speed = isDeleting.value ? 50 : 100;

  if (!isDeleting.value && currentText.value === currentRole) {
    speed = 2000;
    isDeleting.value = true;
  } else if (isDeleting.value && currentText.value === "") {
    isDeleting.value = false;
    currentRoleIndex.value = (currentRoleIndex.value + 1) % roles.length;
    speed = 500;
  }

  setTimeout(typeWriter, speed);
};

// Scroll handlers
const handleScroll = () => {
  scrolled.value = window.scrollY > 50;

  // Check sections visibility
  const sections = ["about", "skills", "experience", "projects", "contact"];
  sections.forEach((section) => {
    const element = document.getElementById(section);
    if (element) {
      const rect = element.getBoundingClientRect();
      const isVisible = rect.top < window.innerHeight - 100;

      switch (section) {
        case "about":
          aboutVisible.value = isVisible;
          break;
        case "skills":
          skillsVisible.value = isVisible;
          break;
        case "experience":
          expVisible.value = isVisible;
          break;
        case "projects":
          projectsVisible.value = isVisible;
          break;
        case "contact":
          contactVisible.value = isVisible;
          break;
      }
    }
  });
};

const scrollToSection = (id: string) => {
  const element = document.getElementById(id);
  if (element) {
    element.scrollIntoView({ behavior: "smooth" });
    mobileMenuOpen.value = false;
  }
};

const scrollToTop = () => {
  window.scrollTo({ top: 0, behavior: "smooth" });
};

const scrollToContact = () => {
  scrollToSection("contact");
};

const openProject = (link: string) => {
  if (link !== "#") window.open(link, "_blank");
};

// Lifecycle
onMounted(() => {
  setTimeout(() => {
    heroVisible.value = true;
    typeWriter();
  }, 100);

  window.addEventListener("scroll", handleScroll);
  handleScroll();
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
});
</script>

<template>
  <div class="relative overflow-x-hidden bg-dark min-h-screen">
    <!-- Navigation -->
    <nav
      class="fixed w-full z-50 nav-blur border-b border-white/10 transition-all duration-300"
      :class="{ 'shadow-lg bg-gray-900/95': scrolled }"
    >
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between items-center h-16">
          <div
            class="font-display font-bold text-xl gradient-text cursor-pointer hover:scale-110 transition-transform"
            @click="scrollToTop"
          >
            AP.
          </div>

          <div class="hidden md:flex space-x-8">
            <a
              v-for="item in navItems"
              :key="item.id"
              @click.prevent="scrollToSection(item.id)"
              href="#"
              class="text-gray-300 hover:text-white transition-colors text-sm font-medium relative group"
            >
              {{ item.label }}
              <span
                class="absolute -bottom-1 left-0 w-0 h-0.5 bg-purple-400 transition-all duration-300 group-hover:w-full"
              ></span>
            </a>
            <button
              @click="scrollToContact"
              class="px-4 py-2 rounded-full bg-white/10 hover:bg-white/20 transition-all text-sm font-medium border border-white/20 hover:border-purple-500/50"
            >
              Contact
            </button>
          </div>

          <button
            @click="mobileMenuOpen = !mobileMenuOpen"
            class="md:hidden text-white p-2"
          >
            <Menu v-if="!mobileMenuOpen" class="w-6 h-6" />
            <X v-else class="w-6 h-6" />
          </button>
        </div>
      </div>

      <!-- Mobile Menu -->
      <Transition
        enter-active-class="transition duration-200 ease-out"
        enter-from-class="transform -translate-y-4 opacity-0"
        enter-to-class="transform translate-y-0 opacity-100"
        leave-active-class="transition duration-150 ease-in"
        leave-from-class="transform translate-y-0 opacity-100"
        leave-to-class="transform -translate-y-4 opacity-0"
      >
        <div
          v-if="mobileMenuOpen"
          class="md:hidden nav-blur border-b border-white/10"
        >
          <div class="px-4 pt-2 pb-4 space-y-2">
            <a
              v-for="item in navItems"
              :key="item.id"
              @click.prevent="scrollToSection(item.id)"
              href="#"
              class="block py-2 text-gray-300 hover:text-white transition-colors"
            >
              {{ item.label }}
            </a>
          </div>
        </div>
      </Transition>
    </nav>

    <!-- Hero Section -->
    <section
      class="relative min-h-screen flex items-center justify-center pt-16 overflow-hidden"
    >
      <div class="absolute inset-0 bg-grid z-0 pointer-events-none"></div>
      <div
        class="absolute top-20 left-10 w-72 h-72 bg-purple-500 rounded-full mix-blend-multiply filter blur-3xl opacity-20 animate-pulse-slow pointer-events-none"
      ></div>
      <div
        class="absolute bottom-20 right-10 w-96 h-96 bg-blue-500 rounded-full mix-blend-multiply filter blur-3xl opacity-20 animate-pulse-slow pointer-events-none"
        style="animation-delay: 2s"
      ></div>

      <div
        class="relative z-10 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center"
      >
        <Transition
          enter-active-class="transition duration-700 ease-out"
          enter-from-class="opacity-0 translate-y-4"
          enter-to-class="opacity-100 translate-y-0"
        >
          <div
            v-if="heroVisible"
            class="inline-flex items-center gap-2 px-4 py-2 rounded-full bg-white/5 border border-white/10 mb-8 cursor-pointer hover:bg-white/10 transition-all hover:scale-105"
          >
            <span
              class="w-2 h-2 rounded-full bg-green-400 animate-pulse"
            ></span>
            <span class="text-sm text-gray-300"
              >Available for opportunities</span
            >
          </div>
        </Transition>

        <Transition
          enter-active-class="transition duration-700 ease-out delay-100"
          enter-from-class="opacity-0 translate-y-4"
          enter-to-class="opacity-100 translate-y-0"
        >
          <h1
            v-if="heroVisible"
            class="font-display text-5xl md:text-7xl lg:text-8xl font-bold mb-6"
          >
            Abhishek <span class="gradient-text">Poojary</span>
          </h1>
        </Transition>

        <Transition
          enter-active-class="transition duration-700 ease-out delay-200"
          enter-from-class="opacity-0 translate-y-4"
          enter-to-class="opacity-100 translate-y-0"
        >
          <div
            v-if="heroVisible"
            class="h-12 md:h-16 mb-8 flex justify-center items-center"
          >
            <span
              class="font-display text-2xl md:text-4xl text-gray-400 typewriter font-light"
              >{{ currentText }}</span
            >
          </div>
        </Transition>

        <Transition
          enter-active-class="transition duration-700 ease-out delay-300"
          enter-from-class="opacity-0 translate-y-4"
          enter-to-class="opacity-100 translate-y-0"
        >
          <p
            v-if="heroVisible"
            class="max-w-2xl mx-auto text-lg text-gray-400 mb-10 leading-relaxed"
          >
            Generative AI Engineer & Full Stack Developer crafting intelligent
            applications. Specializing in LLMs, Flutter mobile apps, and modern
            web architectures. Building the future with AI-powered Apps &
            solutions.
          </p>
        </Transition>

        <Transition
          enter-active-class="transition duration-700 ease-out delay-400"
          enter-from-class="opacity-0 translate-y-4"
          enter-to-class="opacity-100 translate-y-0"
        >
          <div
            v-if="heroVisible"
            class="flex flex-col sm:flex-row gap-4 justify-center items-center"
          >
            <button
              @click="scrollToSection('projects')"
              class="px-8 py-4 rounded-full bg-white text-black font-semibold hover:scale-105 transition-transform flex items-center gap-2 group shadow-lg shadow-purple-500/20"
            >
              View Projects
              <ArrowRight
                class="w-4 h-4 group-hover:translate-x-1 transition-transform"
              />
            </button>
            <a
              href="https://github.com/Abhishek-poojary1"
              target="_blank"
              class="px-8 py-4 rounded-full bg-white/10 border border-white/20 font-semibold hover:bg-white/20 transition-all flex items-center gap-2 hover:border-purple-500/50"
            >
              <Github class="w-4 h-4" /> GitHub
            </a>
            <a
              href="https://www.linkedin.com/in/abhishek-poojary-a50281268"
              target="_blank"
              class="px-8 py-4 rounded-full bg-white/10 border border-white/20 font-semibold hover:bg-white/20 transition-all flex items-center gap-2 hover:border-purple-500/50"
            >
              <Linkedin class="w-4 h-4" /> LinkedIn
            </a>
          </div>
        </Transition>

        <Transition
          enter-active-class="transition duration-700 ease-out delay-500"
          enter-from-class="opacity-0 translate-y-4"
          enter-to-class="opacity-100 translate-y-0"
        >
          <div
            v-if="heroVisible"
            class="mt-16 flex justify-center gap-8 text-gray-500"
          >
            <div
              v-for="stat in stats"
              :key="stat.label"
              class="flex flex-col items-center"
            >
              <span class="font-display text-3xl font-bold text-white">{{
                stat.value
              }}</span>
              <span class="text-sm">{{ stat.label }}</span>
            </div>
          </div>
        </Transition>
      </div>
    </section>

    <!-- About Section -->
    <section id="about" class="py-24 relative">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="grid md:grid-cols-2 gap-16 items-center">
          <div
            :class="[
              'transition-all duration-1000',
              aboutVisible
                ? 'opacity-100 translate-y-0'
                : 'opacity-0 translate-y-8',
            ]"
          >
            <h2 class="font-display text-4xl md:text-5xl font-bold mb-6">
              Bridging <span class="accent-text">AI</span> &
              <span class="gradient-text">Mobile</span>
            </h2>
            <div class="space-y-4 text-gray-400 leading-relaxed">
              <p>
                I'm a
                <strong class="text-white">Generative AI Engineer</strong> and
                <strong class="text-white">Full Stack Developer</strong> based
                in Bengaluru, India. With expertise spanning LLMs, Flutter, and
                modern web frameworks, I build intelligent, production-ready
                applications.
              </p>
              <p>
                At SmartBuild Automation, I integrate AI capabilities into IoT
                ecosystems, improving performance by 40% through architectural
                optimizations. My work spans from RAG-based support systems to
                real-time mobile applications serving hundreds of active users.
              </p>
              <p>
                Whether it's crafting embeddings with FAISS, architecting
                Flutter apps with Clean Architecture, or deploying scalable
                backends with FastAPI—I bring ideas to life with code that
                scales.
              </p>
            </div>

            <div class="mt-8 flex flex-wrap gap-3">
              <span
                v-for="tag in aboutTags"
                :key="tag"
                class="px-4 py-2 rounded-full bg-purple-500/20 text-purple-300 text-sm border border-purple-500/30 hover:bg-purple-500/30 transition-colors cursor-default hover:scale-105 transform"
              >
                {{ tag }}
              </span>
            </div>
          </div>

          <div
            :class="[
              'relative group transition-all duration-1000 delay-200',
              aboutVisible
                ? 'opacity-100 translate-y-0'
                : 'opacity-0 translate-y-8',
            ]"
          >
            <div
              class="absolute inset-0 bg-gradient-to-r from-purple-500 to-blue-500 rounded-2xl blur-2xl opacity-30 group-hover:opacity-50 transition-opacity duration-500"
            ></div>
            <div
              class="relative bg-gray-900 rounded-2xl p-8 border border-white/10"
            >
              <div class="grid grid-cols-2 gap-4">
                <div
                  v-for="(item, index) in expertiseAreas"
                  :key="index"
                  class="skill-card !p-4 hover:!scale-105 transition-transform cursor-pointer glow"
                >
                  <component
                    :is="item.icon"
                    class="w-8 h-8 mb-2"
                    :class="item.color"
                  />
                  <h3 class="font-semibold text-white">{{ item.title }}</h3>
                  <p class="text-xs text-gray-400 mt-1">{{ item.desc }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Skills Section -->
    <section id="skills" class="py-24 bg-white/5">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div
          :class="[
            'text-center mb-16 transition-all duration-1000',
            skillsVisible
              ? 'opacity-100 translate-y-0'
              : 'opacity-0 translate-y-8',
          ]"
        >
          <h2 class="font-display text-4xl md:text-5xl font-bold mb-4">
            Technical <span class="gradient-text">Arsenal</span>
          </h2>
          <p class="text-gray-400 max-w-2xl mx-auto">
            Combining AI innovation with robust engineering across mobile, web,
            and cloud.
          </p>
        </div>

        <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
          <div
            v-for="(category, index) in skillCategories"
            :key="category.title"
            :class="[
              'skill-card relative overflow-hidden group glow transition-all duration-700',
              skillsVisible
                ? 'opacity-100 translate-y-0'
                : 'opacity-0 translate-y-8',
            ]"
            :style="{ transitionDelay: `${index * 100}ms` }"
          >
            <div
              class="absolute top-0 right-0 p-4 opacity-10 group-hover:opacity-20 transition-opacity duration-500"
            >
              <component :is="category.icon" class="w-24 h-24" />
            </div>
            <div class="flex items-center gap-3 mb-4 relative z-10">
              <div class="p-3 rounded-lg" :class="category.bgColor">
                <component
                  :is="category.icon"
                  class="w-6 h-6"
                  :class="category.iconColor"
                />
              </div>
              <h3 class="font-display text-xl font-bold">
                {{ category.title }}
              </h3>
            </div>
            <div class="flex flex-wrap gap-2 relative z-10">
              <span
                v-for="skill in category.skills"
                :key="skill"
                class="px-3 py-1 rounded-full bg-white/5 text-sm border border-white/10 hover:bg-white/10 hover:border-white/30 transition-all cursor-default hover:scale-105 transform"
              >
                {{ skill }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Experience Section -->
    <section id="experience" class="py-24">
      <div class="max-w-5xl mx-auto px-4 sm:px-6 lg:px-8">
        <div
          :class="[
            'text-center mb-16 transition-all duration-1000',
            expVisible
              ? 'opacity-100 translate-y-0'
              : 'opacity-0 translate-y-8',
          ]"
        >
          <h2 class="font-display text-4xl md:text-5xl font-bold mb-4">
            Work <span class="gradient-text">Experience</span>
          </h2>
          <p class="text-gray-400">
            My professional journey in AI and software engineering
          </p>
        </div>

        <div class="relative">
          <div
            class="absolute left-4 md:left-1/2 transform md:-translate-x-1/2 h-full w-1 timeline-line rounded-full"
          ></div>

          <div
            v-for="(job, index) in experiences"
            :key="index"
            :class="[
              'relative mb-12 transition-all duration-700',
              expVisible
                ? 'opacity-100 translate-y-0'
                : 'opacity-0 translate-y-8',
            ]"
            :style="{ transitionDelay: `${index * 200}ms` }"
          >
            <div
              class="absolute left-4 md:left-1/2 transform md:-translate-x-1/2 -translate-x-1/2 w-4 h-4 bg-purple-500 rounded-full border-4 border-gray-900 z-10 shadow-[0_0_20px_rgba(168,85,247,0.5)]"
            ></div>

            <div
              :class="[
                'ml-12 md:ml-0',
                index % 2 === 0
                  ? 'md:mr-[50%] md:pr-12 md:text-right'
                  : 'md:ml-[50%] md:pl-12',
              ]"
            >
              <div
                class="project-card p-6 inline-block text-left w-full"
                :class="{
                  'md:text-left': index % 2 === 1,
                  'md:text-right': index % 2 === 0,
                }"
              >
                <span class="text-purple-400 text-sm font-semibold">{{
                  job.period
                }}</span>
                <h3 class="text-xl font-bold mt-1 mb-2 text-white">
                  {{ job.role }}
                </h3>
                <p class="text-gray-400 text-sm mb-3">{{ job.company }}</p>
                <ul class="text-sm text-gray-300 space-y-1 list-none mb-3">
                  <li
                    v-for="(desc, i) in job.description"
                    :key="i"
                    class="flex items-start gap-2"
                    :class="{ 'md:flex-row-reverse': index % 2 === 0 }"
                  >
                    <span class="text-purple-400">•</span>
                    <span>{{ desc }}</span>
                  </li>
                </ul>
                <div
                  class="flex flex-wrap gap-2"
                  :class="{
                    'md:justify-start': index % 2 === 1,
                    'md:justify-end': index % 2 === 0,
                  }"
                >
                  <span
                    v-for="tech in job.tech"
                    :key="tech"
                    class="text-xs px-2 py-1 rounded bg-white/10 text-gray-300 border border-white/10 hover:bg-purple-500/20 transition-colors"
                  >
                    {{ tech }}
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Projects Section -->
    <!-- Projects Section -->
    <section id="projects" class="py-24 bg-white/5">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div
          :class="[
            'text-center mb-16 transition-all duration-1000',
            projectsVisible
              ? 'opacity-100 translate-y-0'
              : 'opacity-0 translate-y-8',
          ]"
        >
          <h2 class="font-display text-4xl md:text-5xl font-bold mb-4">
            Featured <span class="accent-text">Projects</span>
          </h2>
          <p class="text-gray-400">AI-powered applications and IoT solutions</p>
        </div>

        <div class="grid md:grid-cols-2 gap-8">
          <div
            v-for="(project, index) in projects"
            :key="project.title"
            :class="[
              'project-card group cursor-pointer transition-all duration-700',
              projectsVisible
                ? 'opacity-100 translate-y-0'
                : 'opacity-0 translate-y-8',
            ]"
            :style="{ transitionDelay: `${index * 150}ms` }"
            @click="
              project.title === 'SB Auto & SBA Home'
                ? openAppModal()
                : openProject(project.link)
            "
          >
            <div
              :class="`h-48 bg-gradient-to-br ${project.gradient} relative overflow-hidden`"
            >
              <div
                class="absolute inset-0 bg-black/20 group-hover:bg-black/10 transition-colors duration-500"
              ></div>
              <div class="absolute top-4 right-4">
                <span
                  class="px-3 py-1 bg-black/30 backdrop-blur rounded-full text-xs border border-white/20 text-white"
                >
                  {{ project.tag }}
                </span>
              </div>
              <component
                :is="project.icon"
                class="absolute bottom-4 right-4 w-16 h-16 text-white/20 group-hover:scale-110 group-hover:rotate-12 transition-transform duration-500"
              />
            </div>

            <div class="p-6">
              <h3
                class="text-2xl font-bold mb-2 group-hover:text-purple-400 transition-colors"
              >
                {{ project.title }}
              </h3>
              <p class="text-gray-400 mb-6 text-sm leading-relaxed">
                {{ project.description }}
              </p>

              <div class="flex flex-wrap gap-2 mb-6">
                <span
                  v-for="tech in project.tech"
                  :key="tech"
                  class="text-xs px-3 py-1 rounded-full bg-white/5 border border-white/10 group-hover:border-purple-500/30 transition-colors"
                >
                  {{ tech }}
                </span>
              </div>

              <!-- Simple Link -->
              <div
                class="flex items-center text-purple-400 text-sm font-medium group/link"
              >
                <span class="group-hover/link:underline underline-offset-4"
                  >View Project</span
                >
                <ArrowRight
                  class="w-4 h-4 ml-2 group-hover/link:translate-x-1 transition-transform"
                />
              </div>
            </div>
          </div>

          <!-- GitHub More Projects Card -->
          <div
            :class="[
              'project-card group cursor-pointer transition-all duration-700 border-dashed border-2 border-white/20 hover:border-purple-500/50',
              projectsVisible
                ? 'opacity-100 translate-y-0'
                : 'opacity-0 translate-y-8',
            ]"
            :style="{ transitionDelay: `${projects.length * 150}ms` }"
            @click="openProject('https://github.com/Abhishek-poojary1')"
          >
            <div
              class="h-full min-h-[320px] flex flex-col items-center justify-center p-8 text-center relative overflow-hidden"
            >
              <!-- Background decoration -->
              <div
                class="absolute inset-0 bg-gradient-to-br from-purple-600/10 to-blue-600/10 opacity-0 group-hover:opacity-100 transition-opacity duration-500"
              ></div>

              <div
                class="relative z-10 transform group-hover:scale-110 transition-transform duration-300"
              >
                <div
                  class="w-20 h-20 rounded-full bg-white/5 border border-white/10 flex items-center justify-center mb-6 group-hover:bg-white/10 transition-colors"
                >
                  <Github class="w-10 h-10 text-white/80" />
                </div>
              </div>

              <h3
                class="text-2xl font-bold mb-3 relative z-10 group-hover:text-purple-400 transition-colors"
              >
                More Projects
              </h3>
              <p class="text-gray-400 text-sm mb-6 relative z-10 max-w-xs">
                Explore my complete portfolio of open-source projects,
                experiments, and contributions on GitHub.
              </p>

              <div
                class="flex items-center text-purple-400 text-sm font-medium relative z-10"
              >
                <span class="group-hover:underline underline-offset-4"
                  >View GitHub Profile</span
                >
                <ArrowRight
                  class="w-4 h-4 ml-2 group-hover:translate-x-1 transition-transform"
                />
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- App Selection Modal -->
      <Teleport to="body">
        <Transition
          enter-active-class="transition duration-300 ease-out"
          enter-from-class="opacity-0"
          enter-to-class="opacity-100"
          leave-active-class="transition duration-200 ease-in"
          leave-from-class="opacity-100"
          leave-to-class="opacity-0"
        >
          <div
            v-if="showAppModal"
            class="fixed inset-0 z-[100] flex items-center justify-center p-4 bg-black/80 backdrop-blur-sm"
            @click.self="closeAppModal"
          >
            <Transition
              enter-active-class="transition duration-300 ease-out delay-100"
              enter-from-class="opacity-0 scale-95 translate-y-4"
              enter-to-class="opacity-100 scale-100 translate-y-0"
              leave-active-class="transition duration-200 ease-in"
              leave-from-class="opacity-100 scale-100 translate-y-0"
              leave-to-class="opacity-0 scale-95 translate-y-4"
            >
              <div
                v-if="showAppModal"
                ref="modalRef"
                class="bg-gray-900 border border-white/10 rounded-3xl p-8 max-w-4xl w-full shadow-2xl shadow-purple-500/20 relative overflow-hidden"
              >
                <!-- Close button -->
                <button
                  @click="closeAppModal"
                  class="absolute top-4 right-4 p-2 rounded-full bg-white/5 hover:bg-white/10 transition-colors z-20 group"
                >
                  <X class="w-5 h-5 text-gray-400 group-hover:text-white" />
                </button>

                <!-- Decorative background -->
                <div
                  class="absolute top-0 left-0 w-full h-32 bg-gradient-to-b from-purple-500/10 to-transparent pointer-events-none"
                ></div>

                <!-- Header -->
                <div class="text-center mb-10 relative z-10">
                  <div
                    class="inline-flex items-center gap-2 px-4 py-2 rounded-full bg-white/5 border border-white/10 mb-4"
                  >
                    <Smartphone class="w-4 h-4 text-purple-400" />
                    <span class="text-sm text-gray-300">Live on App Store</span>
                  </div>
                  <h3
                    class="font-display text-3xl md:text-4xl font-bold text-white mb-2"
                  >
                    Choose Your App
                  </h3>
                  <p class="text-gray-400">
                    Select which automation app you'd like to explore
                  </p>
                </div>

                <!-- Two Cards Grid -->
                <div class="grid md:grid-cols-2 gap-6 relative z-10">
                  <!-- SB Auto Card -->
                  <div
                    class="group relative bg-white/5 border border-white/10 rounded-2xl p-6 hover:bg-white/10 hover:border-purple-500/50 transition-all duration-300 hover:-translate-y-1 cursor-pointer"
                    @click="openProject(appLinks.sbAuto.appStore)"
                  >
                    <div
                      class="absolute inset-0 bg-gradient-to-br from-blue-500/20 to-cyan-500/20 opacity-0 group-hover:opacity-100 transition-opacity rounded-2xl"
                    ></div>

                    <div class="relative z-10">
                      <div
                        class="w-16 h-16 rounded-2xl bg-gradient-to-br from-blue-500 to-cyan-500 flex items-center justify-center mb-4 group-hover:scale-110 transition-transform shadow-lg shadow-blue-500/20"
                      >
                        <Home class="w-8 h-8 text-white" />
                      </div>

                      <h4 class="text-2xl font-bold text-white mb-2">
                        SB Auto
                      </h4>
                      <p class="text-gray-400 text-sm mb-6">
                        Complete home automation solution with advanced device
                        controls and real-time monitoring.
                      </p>

                      <div class="space-y-3">
                        <a
                          :href="appLinks.sbAuto.appStore"
                          target="_blank"
                          class="flex items-center justify-center gap-2 w-full py-3 rounded-xl bg-white text-black font-semibold hover:scale-[1.02] transition-transform"
                          @click.stop
                        >
                          <svg
                            class="w-5 h-5"
                            viewBox="0 0 24 24"
                            fill="currentColor"
                          >
                            <path
                              d="M18.71 19.5c-.83 1.24-1.71 2.45-3.05 2.47-1.34.03-1.77-.79-3.29-.79-1.53 0-2 .77-3.27.82-1.31.05-2.3-1.32-3.14-2.53C4.25 17 2.94 12.45 4.7 9.39c.87-1.52 2.43-2.48 4.12-2.51 1.28-.02 2.5.87 3.29.87.78 0 2.26-1.07 3.81-.91.65.03 2.47.26 3.64 1.98-.09.06-2.17 1.28-2.15 3.81.03 3.02 2.65 4.03 2.68 4.04-.03.07-.42 1.44-1.38 2.83M13 3.5c.73-.83 1.94-1.46 2.94-1.5.13 1.17-.34 2.35-1.04 3.19-.69.85-1.83 1.51-2.95 1.42-.15-1.15.41-2.35 1.05-3.11z"
                            />
                          </svg>
                          App Store
                        </a>
                        <a
                          :href="appLinks.sbAuto.playStore"
                          target="_blank"
                          class="flex items-center justify-center gap-2 w-full py-3 rounded-xl bg-white/10 border border-white/20 text-white font-semibold hover:bg-white/20 transition-all"
                          @click.stop
                        >
                          <svg
                            class="w-5 h-5"
                            viewBox="0 0 24 24"
                            fill="currentColor"
                          >
                            <path
                              d="M3,20.5V3.5C3,2.91 3.34,2.39 3.84,2.15L13.69,12L3.84,21.85C3.34,21.6 3,21.09 3,20.5M16.81,15.12L6.05,21.34L14.54,12.85L16.81,15.12M20.16,10.81C20.5,11.08 20.75,11.5 20.75,12C20.75,12.5 20.53,12.9 20.18,13.18L17.89,14.5L15.39,12L17.89,9.5L20.16,10.81M6.05,2.66L16.81,8.88L14.54,11.15L6.05,2.66Z"
                            />
                          </svg>
                          Play Store
                        </a>
                      </div>
                    </div>
                  </div>

                  <!-- SBA Home Card -->
                  <div
                    class="group relative bg-white/5 border border-white/10 rounded-2xl p-6 hover:bg-white/10 hover:border-pink-500/50 transition-all duration-300 hover:-translate-y-1 cursor-pointer"
                    @click="openProject(appLinks.sbaHome.appStore)"
                  >
                    <div
                      class="absolute inset-0 bg-gradient-to-br from-pink-500/20 to-purple-500/20 opacity-0 group-hover:opacity-100 transition-opacity rounded-2xl"
                    ></div>

                    <div class="relative z-10">
                      <div
                        class="w-16 h-16 rounded-2xl bg-gradient-to-br from-pink-500 to-purple-500 flex items-center justify-center mb-4 group-hover:scale-110 transition-transform shadow-lg shadow-pink-500/20"
                      >
                        <Smartphone class="w-8 h-8 text-white" />
                      </div>

                      <h4 class="text-2xl font-bold text-white mb-2">
                        SBA Home
                      </h4>
                      <p class="text-gray-400 text-sm mb-6">
                        Smart home automation with device grouping, routines,
                        and cloud synchronization.
                      </p>

                      <div class="space-y-3">
                        <a
                          :href="appLinks.sbaHome.appStore"
                          target="_blank"
                          class="flex items-center justify-center gap-2 w-full py-3 rounded-xl bg-white text-black font-semibold hover:scale-[1.02] transition-transform"
                          @click.stop
                        >
                          <svg
                            class="w-5 h-5"
                            viewBox="0 0 24 24"
                            fill="currentColor"
                          >
                            <path
                              d="M18.71 19.5c-.83 1.24-1.71 2.45-3.05 2.47-1.34.03-1.77-.79-3.29-.79-1.53 0-2 .77-3.27.82-1.31.05-2.3-1.32-3.14-2.53C4.25 17 2.94 12.45 4.7 9.39c.87-1.52 2.43-2.48 4.12-2.51 1.28-.02 2.5.87 3.29.87.78 0 2.26-1.07 3.81-.91.65.03 2.47.26 3.64 1.98-.09.06-2.17 1.28-2.15 3.81.03 3.02 2.65 4.03 2.68 4.04-.03.07-.42 1.44-1.38 2.83M13 3.5c.73-.83 1.94-1.46 2.94-1.5.13 1.17-.34 2.35-1.04 3.19-.69.85-1.83 1.51-2.95 1.42-.15-1.15.41-2.35 1.05-3.11z"
                            />
                          </svg>
                          App Store
                        </a>
                        <a
                          :href="appLinks.sbaHome.playStore"
                          target="_blank"
                          class="flex items-center justify-center gap-2 w-full py-3 rounded-xl bg-white/10 border border-white/20 text-white font-semibold hover:bg-white/20 transition-all"
                          @click.stop
                        >
                          <svg
                            class="w-5 h-5"
                            viewBox="0 0 24 24"
                            fill="currentColor"
                          >
                            <path
                              d="M3,20.5V3.5C3,2.91 3.34,2.39 3.84,2.15L13.69,12L3.84,21.85C3.34,21.6 3,21.09 3,20.5M16.81,15.12L6.05,21.34L14.54,12.85L16.81,15.12M20.16,10.81C20.5,11.08 20.75,11.5 20.75,12C20.75,12.5 20.53,12.9 20.18,13.18L17.89,14.5L15.39,12L17.89,9.5L20.16,10.81M6.05,2.66L16.81,8.88L14.54,11.15L6.05,2.66Z"
                            />
                          </svg>
                          Play Store
                        </a>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- Footer hint -->
                <div
                  class="mt-8 text-center text-gray-500 text-sm relative z-10"
                >
                  Click outside or press ESC to close
                </div>
              </div>
            </Transition>
          </div>
        </Transition>
      </Teleport>
    </section>

    <!-- Education & Contact -->
    <section id="contact" class="py-24 relative overflow-hidden">
      <div
        class="absolute inset-0 bg-gradient-to-b from-purple-900/20 to-transparent pointer-events-none"
      ></div>

      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
        <div class="grid md:grid-cols-2 gap-16 mb-16">
          <!-- Education -->
          <div
            :class="[
              'transition-all duration-1000',
              contactVisible
                ? 'opacity-100 translate-y-0'
                : 'opacity-0 translate-y-8',
            ]"
          >
            <h3
              class="font-display text-2xl font-bold mb-6 flex items-center gap-2 text-white"
            >
              <GraduationCap class="text-purple-400 w-6 h-6" />
              Education
            </h3>
            <div class="space-y-4">
              <div
                v-for="edu in education"
                :key="edu.degree"
                class="skill-card hover:!border-purple-500/30"
              >
                <h4 class="font-bold text-lg text-white">{{ edu.degree }}</h4>
                <p class="text-gray-400">{{ edu.school }}</p>
                <p class="text-sm text-purple-400 mt-1">{{ edu.year }}</p>
              </div>
            </div>

            <div class="mt-8">
              <h3
                class="font-display text-2xl font-bold mb-4 flex items-center gap-2 text-white"
              >
                <Award class="text-yellow-400 w-6 h-6" />
                Achievements
              </h3>
              <ul class="space-y-3">
                <li
                  v-for="achievement in achievements"
                  :key="achievement"
                  class="flex items-start gap-3 text-gray-400 text-sm"
                >
                  <Trophy
                    class="w-4 h-4 text-yellow-400 flex-shrink-0 mt-0.5"
                  />
                  <span>{{ achievement }}</span>
                </li>
              </ul>
            </div>
          </div>

          <!-- Contact -->
          <div
            :class="[
              'text-center md:text-left transition-all duration-1000 delay-200',
              contactVisible
                ? 'opacity-100 translate-y-0'
                : 'opacity-0 translate-y-8',
            ]"
          >
            <h2 class="font-display text-4xl md:text-5xl font-bold mb-6">
              Let's Build <span class="gradient-text">Together</span>
            </h2>
            <p class="text-xl text-gray-400 mb-10">
              Open for opportunities in GenAI, Mobile & Web Development
            </p>

            <div class="flex flex-col sm:flex-row gap-4 mb-12">
              <a
                href="mailto:poojaryabhishek26@gmail.com"
                class="px-8 py-4 rounded-full bg-white text-black font-semibold hover:scale-105 transition-transform flex items-center justify-center gap-2 shadow-lg shadow-purple-500/20"
              >
                <Mail class="w-5 h-5" />
                poojaryabhishek26@gmail.com
              </a>
              <a
                href="tel:+919945726449"
                class="px-8 py-4 rounded-full bg-white/10 border border-white/20 font-semibold hover:bg-white/20 transition-all flex items-center justify-center gap-2 hover:border-purple-500/50 whitespace-nowrap"
              >
                <Phone class="w-5 h-5" />
                +91 99457 26449
              </a>
            </div>

            <div class="flex justify-center md:justify-start gap-4">
              <a
                v-for="social in socials"
                :key="social.name"
                :href="social.url"
                target="_blank"
                class="p-4 rounded-full bg-white/5 hover:bg-white/10 transition-all border border-white/10 hover:border-purple-500/50 transform hover:scale-110"
              >
                <component :is="social.icon" class="w-6 h-6" />
              </a>
            </div>
          </div>
        </div>

        <!-- Footer -->
        <div
          class="pt-8 border-t border-white/10 text-gray-500 text-sm text-center"
        >
          <p>© 2024 Abhishek Poojary. Built with Vue 3 & Tailwind CSS.</p>
        </div>
      </div>
    </section>
  </div>
</template>
