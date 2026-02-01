# chosotn-
ing

"use client";

import { useEffect, useState } from "react";

/* ---------- Language ---------- */
type Lang = "ko" | "en" | "zh-Hans" | "zh-Hant" | "ja";
const LANG_KEY = "chsotn_lang";
const CONSENT_KEY = "chsotn_user_consent_v1";

function getSavedLang(): Lang {
  if (typeof window === "undefined") return "ko";
  const saved = localStorage.getItem(LANG_KEY) as Lang | null;
  if (saved) return saved;

  const nav = navigator.language.toLowerCase();
  if (nav.startsWith("en")) return "en";
  if (nav.startsWith("zh")) return "zh-Hans";
  if (nav.startsWith("ja")) return "ja";
  return "ko";
}

function saveLang(lang: Lang) {
  localStorage.setItem(LANG_KEY, lang);
}

function getConsent() {
  return localStorage.getItem(CONSENT_KEY);
}

function setConsent(v: "accepted" | "declined") {
  localStorage.setItem(CONSENT_KEY, v);
}

/* ---------- TEXT ---------- */
const TEXT: Record<Lang, any> = {
  ko: {
    title: "글로벌 학습을 위한 자막 · 번역 인프라",
    desc:
      "chsotn은 콘텐츠를 소유하지 않습니다. 공식 제휴를 통해 외국어 자막 서비스를 제공하며, 강의와 지식을 전 세계로 연결합니다.",
    start: "시작하기",
    partner: "제휴 문의",
    consentTitle: "당신의 선택을 존중합니다",
    consent:
      `사용자 여러분,
당신의 정보에 대한 진정한 선택이 필요합니다.

우리는 정보를 요구하기 전에,
당신의 권리를 먼저 존중합니다.

개인정보는 언제나
사용자 본인의 것입니다.

형식적인 동의가 아닌,
당신의 진정한 허락이 필요합니다.

— chsotn / HDK company`
  },

  en: {
    title: "Subtitle & Translation Infrastructure for Global Learning",
    desc:
      "chsotn does not own content. Through official partnerships, we connect knowledge to the world.",
    start: "Get Started",
    partner: "Partnership Inquiry",
    consentTitle: "We Respect Your Choice",
    consent:
      `Your data belongs to you.

We do not collect information lightly.
Only with your permission, we connect value globally.

— chsotn / HDK company`
  },

  "zh-Hans": {
    title: "面向全球学习的字幕与翻译基础设施",
    desc: "chsotn 不拥有任何内容，通过官方合作连接全球知识。",
    start: "开始使用",
    partner: "合作咨询",
    consentTitle: "我们尊重您的选择",
    consent:
      `您的信息始终属于您本人。
仅在您许可的范围内，我们连接全球价值。`
  },

  "zh-Hant": {
    title: "面向全球學習的字幕與翻譯基礎設施",
    desc: "chsotn 不擁有任何內容，透過官方合作連結全球知識。",
    start: "開始使用",
    partner: "合作洽詢",
    consentTitle: "我們尊重您的選擇",
    consent:
      `您的資訊始終屬於您本人。
僅在您同意的範圍內，我們連結全球價值。`
  },

  ja: {
    title: "グローバル学習のための字幕・翻訳インフラ",
    desc:
      "chsotnはコンテンツを所有せず、公式パートナーシップを通じて知識を世界へつなげます。",
    start: "はじめる",
    partner: "提携のお問い合わせ",
    consentTitle: "あなたの選択を尊重します",
    consent:
      `あなたの情報は、常にあなたのものです。
同意のない取得は行いません。`
  }
};

export default function Page() {
  const [lang, setLang] = useState<Lang>("ko");
  const [consent, setConsentState] = useState<string | null>(null);

  useEffect(() => {
    setLang(getSavedLang());
    setConsentState(getConsent());
  }, []);

  const t = TEXT[lang];

  return (
    <main className="min-h-screen bg-neutral-950 text-white p-8">
      <header className="flex justify-between items-center mb-10">
        <h1 className="text-xl font-bold">chsotn</h1>

        <select
          value={lang}
          onChange={(e) => {
            const v = e.target.value as Lang;
            setLang(v);
            saveLang(v);
          }}
          className="bg-neutral-900 border border-neutral-700 px-3 py-2 rounded"
        >
          <option value="ko">한국어</option>
          <option value="en">English</option>
          <option value="zh-Hans">中文(简体)</option>
          <option value="zh-Hant">中文(繁體)</option>
          <option value="ja">日本語</option>
        </select>
      </header>

      <section className="max-w-3xl">
        <h2 className="text-3xl font-semibold">{t.title}</h2>
        <p className="mt-4 text-neutral-300">{t.desc}</p>

        <div className="mt-6 flex gap-4">
          <button className="bg-white text-black px-5 py-3 rounded">
            {t.start}
          </button>
          <button className="border border-neutral-700 px-5 py-3 rounded">
            {t.partner}
          </button>
        </div>
      </section>

      {!consent && (
        <div className="fixed inset-0 bg-black/80 flex items-center justify-center">
          <div className="bg-neutral-900 border border-neutral-700 rounded-xl p-6 max-w-lg">
            <h3 className="text-lg font-semibold mb-3">{t.consentTitle}</h3>
            <pre className="text-sm whitespace-pre-wrap text-neutral-300 mb-4">
              {t.consent}
            </pre>
            <button
              className="bg-white text-black px-4 py-2 rounded"
              onClick={() => {
                setConsent("accepted");
                setConsentState("accepted");
              }}
            >
              {t.start}
            </button>
          </div>
        </div>
      )}
    </main>
  );
}

