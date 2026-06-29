"use strict";
/*
 * common_actors.js — 共通出演者ファインダー（ブラウザ単体・サーバ不要）のロジック＋UI。
 * Wikidata(SPARQL/Action API)・各言語版WikipediaにCORSで直接アクセスする。
 */

/* ===== 定数・設定 ===== */
const WD_API = "https://www.wikidata.org/w/api.php";
const SPARQL = "https://query.wikidata.org/sparql";
const WEAK_THRESHOLD = 12;
const WORK_CLASSES = ["Q2431196", "Q17537576", "Q24856", "Q196600", "Q21198342"];
const WIKILINK = /\[\[([^\[\]]+?)\]\]/g;
const MAIN_TMPL = /\{\{[Mm]ain[^}]*?\|([^}|]+)/g;

// 役割キー → [[Wikidataプロパティ, 表示ラベル], ...]
const ROLE_PROPS = {
  cast:           [["P161", "出演"]],
  voice:          [["P725", "声優"]],
  director:       [["P57", "監督"]],
  writer:         [["P58", "脚本"]],
  music:          [["P86", "音楽"]],
  producer:       [["P162", "製作"]],
  editor:         [["P1040", "編集"]],
  cinematography: [["P344", "撮影"]],
};
const DEFAULT_ROLES = ["cast", "voice"];

// Wikipedia補強で横断できる言語版（チェックボックスを動的生成）
const LANG_LABELS = {
  ja: "日本語", en: "英語", zh: "中国語", ko: "韓国語",
  hi: "ヒンディー", ta: "タミル", te: "テルグ", ml: "マラヤーラム",
};
const DEFAULT_LANGS = ["ja", "en", "zh"];

// 言語プリセット（用途別にワンクリックでチェックを設定）
const LANG_PRESETS = {
  "日本": ["ja"],
  "英語圏": ["ja", "en"],
  "中国語圏": ["ja", "en", "zh"],
  "韓国": ["ja", "en", "ko"],
  "インド": ["ja", "en", "hi", "ta", "te", "ml"],
};

// 言語版ごとのキャスト解析設定（section=キャスト見出し, infobox=出演系引数,
//   va=声優マーカー直後リンク, subKw=登場人物記事キーワード, ns=除外名前空間）
const WIKI_CONFIG = {
  ja: {
    section: /(キャスト|出演|声の出演|声優|登場人物|登場キャラクター|キャラクター)/,
    infobox: /^\s*\|\s*(出演者|声の出演|声優|ナレーター|主演)\s*=/,
    // 声優マーカー（アニメ等）。「演」は"演じる"＝出演の意味なので含めない
    va: /(?:声(?:優)?|CV|cv)\s*[-‐–—−:：]\s*((?:\[\[[^\]]+\]\][^\[\n]{0,4}){1,6})/g,
    // 日本語吹替の声優マーカー（インライン「吹替 - [[名]]」）。出演から除外して声優扱い
    dub: /(?:日本語吹替|吹替え?|吹き替え)\s*[-‐–—−:：]\s*((?:\[\[[^\]]+\]\][^\[\n]{0,10}){1,4})/g,
    // 「== 日本語吹き替え ==」等の見出し（表形式の吹替表）。中の人物は声優として拾う
    dubSection: /(吹替|吹き替え)/,
    subKw: ["登場人物", "キャラクター"],
    ns: /^(Category|カテゴリ|File|ファイル|画像|Image|Template|Wikipedia|Help|Portal|プロジェクト|特別):/i,
  },
  en: {
    section: /(Cast|Voice|Starring|Casting|Characters)/i,
    infobox: /^\s*\|\s*(starring|voices|voice|narrated_by)\s*=/i,
    va: /(?:voiced by|portrayed by|played by)\s+((?:\[\[[^\]]+\]\][^\[\n]{0,4}){1,6})/gi,
    subKw: ["characters"],
    ns: /^(Category|File|Image|Template|Wikipedia|Help|Portal|Module|Special|wikt|s|c):/i,
  },
  zh: {
    section: /(演員|演员|角色|配音|主演|聲演|声演|登場人物|登场人物)/,
    infobox: /^\s*\|\s*(主演|配音|starring)\s*=/,
    va: /(?:配音|聲優|声优|聲|声)\s*[-‐–—−:：]\s*((?:\[\[[^\]]+\]\][^\[\n]{0,4}){1,6})/g,
    subKw: ["角色", "登場人物", "登场人物"],
    ns: /^(Category|分類|分类|File|檔案|文件|Image|圖像|图像|Template|模板|Wikipedia|Help|Portal|Special|Module):/i,
  },
  ko: {
    section: /(출연|등장인물|배우|성우|주연|캐스트)/,
    infobox: /^\s*\|\s*(출연|주연|성우|starring)\s*=/,
    va: /(?:성우|목소리)\s*[-‐–—−:：]\s*((?:\[\[[^\]]+\]\][^\[\n]{0,4}){1,6})/g,
    subKw: ["등장인물"],
    ns: /^(분류|파일|그림|틀|위키백과|도움말|특수|Category|File|Template):/i,
  },
  hi: {
    section: /(कलाकार|पात्र|मुख्य कलाकार|कास्ट|Cast)/i,
    infobox: /^\s*\|\s*(कलाकार|मुख्य_कलाकार|starring)\s*=/i,
    va: null,
    subKw: ["पात्र"],
    ns: /^(श्रेणी|चित्र|फ़ाइल|साँचा|विकिपीडिया|Category|File|Template):/i,
  },
  ta: {
    section: /(நடிகர்கள்|வார்ப்பு|பாத்திரங்கள்|நடித்தோர்|குரல்|Cast)/i,
    infobox: /^\s*\|\s*(நடிகர்கள்|starring)\s*=/i,
    va: null,
    subKw: ["பாத்திரங்கள்"],
    ns: /^(பகுப்பு|படிமம்|வார்ப்புரு|கோப்பு|Category|File|Template):/i,
  },
  te: {
    section: /(నటవర్గం|తారాగణం|పాత్రలు|నటీనటులు|Cast)/i,
    infobox: /^\s*\|\s*(నటవర్గం|తారాగణం|starring)\s*=/i,
    va: null,
    subKw: ["పాత్రలు"],
    ns: /^(వర్గం|దస్త్రం|మూస|బొమ్మ|Category|File|Template):/i,
  },
  ml: {
    section: /(അഭിനേതാക്കൾ|താരനിര|കഥാപാത്രങ്ങൾ|അഭിനയം|Cast)/i,
    infobox: /^\s*\|\s*(അഭിനേതാക്കൾ|താരനിര|starring)\s*=/i,
    va: null,
    subKw: ["കഥാപാത്രങ്ങൾ"],
    ns: /^(വർഗ്ഗം|പ്രമാണം|ഫലകം|ചിത്രം|Category|File|Template):/i,
  },
};

/* ===== HTTP（429/503・タイムアウトをバックオフ再試行）===== */
const sleep = ms => new Promise(r => setTimeout(r, ms));

async function httpGet(url) {
  for (let a = 0; a < 5; a++) {
    let resp;
    try { resp = await fetch(url); }
    catch (e) { if (a < 4) { await sleep(Math.min(2 ** a * 1000, 20000)); continue; } throw e; }
    if ((resp.status === 429 || resp.status === 503) && a < 4) {
      const ra = parseFloat(resp.headers.get("Retry-After")) || 2 ** a;
      await sleep(Math.min(ra * 1000, 20000)); continue;
    }
    if (!resp.ok) throw new Error("HTTP " + resp.status);
    return resp;
  }
}

// セッション内キャッシュ（リロードするまで保持）。同じ検索を高速化する。
const _sparqlCache = new Map();
const _jsonCache = new Map();

async function sparql(query) {
  if (_sparqlCache.has(query)) return _sparqlCache.get(query);
  const body = "query=" + encodeURIComponent(query) + "&format=json";
  for (let a = 0; a < 5; a++) {
    let resp;
    try {
      resp = await fetch(SPARQL, {
        method: "POST",
        headers: { "Content-Type": "application/x-www-form-urlencoded",
                   "Accept": "application/sparql-results+json" },
        body,
      });
    } catch (e) { if (a < 4) { await sleep(Math.min(2 ** a * 1000, 20000)); continue; } throw e; }
    if ((resp.status === 429 || resp.status === 503) && a < 4) {
      const ra = parseFloat(resp.headers.get("Retry-After")) || 2 ** a;
      await sleep(Math.min(ra * 1000, 20000)); continue;
    }
    if (!resp.ok) throw new Error("SPARQL " + resp.status);
    const bindings = (await resp.json()).results.bindings;
    _sparqlCache.set(query, bindings);
    return bindings;
  }
}

function apiURL(base, params) {
  const p = new URLSearchParams(Object.assign({}, params, { format: "json", origin: "*" }));
  return base + "?" + p.toString();
}

// Action API(JSON) もURL単位でキャッシュ
async function getJSON(url) {
  if (_jsonCache.has(url)) return _jsonCache.get(url);
  const j = await (await httpGet(url)).json();
  _jsonCache.set(url, j);
  return j;
}
const qval = b => b.value.split("/").pop();

/* ===== タイトル→QID 解決 ===== */
async function wbsearch(title, lang) {
  const url = apiURL(WD_API, { action: "wbsearchentities", search: title,
    language: lang, uselang: lang, type: "item", limit: 7 });
  const j = await getJSON(url);
  let res = j.search || [];
  if (!res.length && lang !== "en") return wbsearch(title, "en");
  return res;
}

async function filterWorkQids(qids) {
  if (!qids.length) return new Set();
  const values = qids.map(q => "wd:" + q).join(" ");
  const classes = WORK_CLASSES.map(c => "wd:" + c).join(" ");
  const rows = await sparql(
    `SELECT DISTINCT ?item WHERE { VALUES ?item { ${values} } `
    + `VALUES ?cls { ${classes} } ?item wdt:P31/wdt:P279* ?cls . `
    + `FILTER NOT EXISTS { ?item wdt:P31 wd:Q4167410 } `    // 曖昧さ回避ページを除外
    + `FILTER NOT EXISTS { ?item wdt:P31 wd:Q13406463 } }`); // 一覧記事を除外
  return new Set(rows.map(b => qval(b.item)));
}

// 末尾の曖昧さ回避括弧を除去:「X (2009年の映画)」「X（アニメ）」→「X」
function stripParen(title) {
  let base = title.trim();
  for (;;) {
    const s = base.replace(/[（(][^（）()]*[）)]\s*$/, "").trim();
    if (s === base) break;
    base = s;
  }
  return base;
}

// 表記ゆれ吸収用の検索語（区切り文字を入れ替えた形。トリミングはしない）
function normalizedVariants(title) {
  const out = [];
  const add = s => { s = (s || "").trim(); if (s && !out.includes(s)) out.push(s); };
  const base = stripParen(title);
  add(title); add(base);
  const SEP = /[/／・:：]/g;   // 「アベンジャーズ/エンドゲーム」→「・」形など
  add(base.replace(SEP, "・"));
  add(base.replace(SEP, "/"));
  add(base.replace(SEP, " "));
  add(base.replace(SEP, ""));
  return out;
}

// 最後の手段: 空白で区切って末尾語を順に落とす（副題対策。/ や ・ では割らない）
function trimVariants(title) {
  const out = [];
  const add = s => { s = (s || "").trim(); if (s && !out.includes(s)) out.push(s); };
  const parts = stripParen(title).split(/[\s　]+/).filter(Boolean);
  for (let k = parts.length - 1; k > 0; k--) add(parts.slice(0, k).join(" "));
  return out;
}

// Wikipedia記事タイトル → Wikidata QID（曖昧さ回避括弧つきの記事名も正確に解決）
async function resolveViaWiki(title, wikiLang) {
  const url = apiURL(`https://${wikiLang}.wikipedia.org/w/api.php`, {
    action: "query", titles: title, prop: "pageprops",
    ppprop: "wikibase_item", redirects: 1, formatversion: 2 });
  const j = await getJSON(url);
  const pages = (j.query && j.query.pages) || [];
  if (!pages.length || pages[0].missing) return null;
  return (pages[0].pageprops && pages[0].pageprops.wikibase_item) || null;
}

async function getEntityInfo(qid, lang) {
  const url = apiURL(WD_API, { action: "wbgetentities", ids: qid,
    props: "labels|descriptions", languages: `${lang}|en` });
  const j = await getJSON(url);
  const e = (j.entities || {})[qid] || {};
  const lbl = ((e.labels && (e.labels[lang] || e.labels.en)) || {}).value || qid;
  const dsc = ((e.descriptions && (e.descriptions[lang] || e.descriptions.en)) || {}).value || "";
  return { label: lbl, description: dsc };
}

// Wikipedia全文検索(CirrusSearch)で記事を探しQID配列を返す。
// 中黒(・)やスペースの揺れに強い（例:「スターウォーズ」→「スター・ウォーズ」）。
async function searchWiki(title, wikiLang) {
  const url = apiURL(`https://${wikiLang}.wikipedia.org/w/api.php`, {
    action: "query", generator: "search", gsrsearch: title, gsrlimit: 6,
    gsrnamespace: 0, prop: "pageprops", ppprop: "wikibase_item", formatversion: 2 });
  let j;
  try { j = await getJSON(url); } catch (e) { return []; }
  const pages = (j.query && j.query.pages) || [];
  pages.sort((a, b) => (a.index || 99) - (b.index || 99));   // 検索順位
  return pages
    .map(p => ({ qid: p.pageprops && p.pageprops.wikibase_item, title: p.title }))
    .filter(x => x.qid);
}

// 区切り文字・大小文字を無視した比較用キー
const normKey = s => (s || "").replace(/[\s　・:：/／〜~‐–—-]/g, "").toLowerCase();

async function infoFromQid(qid, lang, matched) {
  const info = await getEntityInfo(qid, lang);
  return { qid, label: info.label, description: info.description,
           is_work: true, matched_query: matched };
}

async function resolveQid(title, lang) {
  // 「(2009年の映画)」等の括弧内にある年を、候補の絞り込みヒントに使う
  const ym = title.match(/[（(][^）)]*?(\d{4})[^）)]*[）)]/);
  const year = ym ? ym[1] : null;
  let fallback = null;

  // Tier 1: Wikipedia記事タイトルとして完全一致で解決（記事名そのもの・括弧つきに強い）
  for (const wl of [lang, "en"]) {
    const qid = await resolveViaWiki(title, wl);
    if (qid && (await filterWorkQids([qid])).has(qid)) return infoFromQid(qid, lang, title);
  }

  // Wikidata検索を試す共通処理（作品型優先・年ヒスト考慮）
  const tryWb = async variants => {
    for (const v of variants) {
      const cands = await wbsearch(v, lang);
      if (!cands.length) continue;
      const works = await filterWorkQids(cands.map(c => c.id));
      const score = c => !works.has(c.id) ? 2 : ((year && (c.description || "").includes(year)) ? -1 : 0);
      const ranked = cands.slice().sort((a, b) => score(a) - score(b));
      const top = ranked[0];
      const info = { qid: top.id, label: top.label || "", description: top.description || "",
                     is_work: works.has(top.id), matched_query: v };
      if (works.size) return info;
      if (!fallback) fallback = info;
    }
    return null;
  };

  // Tier 2: Wikidata検索（フル＋区切り表記ゆれ。略称・別名にも強い。トリミングはまだしない）
  let r = await tryWb(normalizedVariants(title));
  if (r) return r;

  // Tier 3: Wikipedia全文検索（中黒・スペースの揺れに強い。例「スターウォーズ」→「スター・ウォーズ」）
  const target = normKey(stripParen(title));
  for (const wl of [lang, "en"]) {
    const hits = await searchWiki(title, wl);
    if (!hits.length) continue;
    const works = await filterWorkQids(hits.map(h => h.qid));
    const wlist = hits.filter(h => works.has(h.qid));
    if (!wlist.length) continue;
    // 記事名が入力と（区切り無視で）一致＞前方一致＞検索順位、の順で選ぶ
    const score = h => {
      const n = normKey(h.title);
      if (n === target) return 0;
      if (n.startsWith(target) || target.startsWith(n)) return 1;
      return 2;
    };
    wlist.sort((a, b) => score(a) - score(b));
    return infoFromQid(wlist[0].qid, lang, title);
  }

  // Tier 4: 最後の手段（空白で末尾を削る副題対策）
  r = await tryWb(trimVariants(title));
  if (r) return r;

  return fallback;
}

/* ===== Wikidata: 役割取得（2段階で監督等も高速）===== */
async function getSeriesMemberQids(qid) {
  const rows = await sparql(
    `SELECT ?work WHERE { { ?work wdt:P179 wd:${qid}. } UNION { wd:${qid} wdt:P527 ?work. } }`);
  return rows.map(b => qval(b.work));
}

async function getPeopleWikidata(qid, lang, props, withSeries) {
  let workQids = [qid];
  if (withSeries) workQids = workQids.concat(await getSeriesMemberQids(qid));
  workQids = [...new Set(workQids)];
  const clauses = props.map(([p, l]) =>
    `{ ?work wdt:${p} ?person. BIND("${l}" AS ?role) }`).join(" UNION ");
  const people = new Map();
  for (let i = 0; i < workQids.length; i += 400) {
    const values = workQids.slice(i, i + 400).map(w => "wd:" + w).join(" ");
    const rows = await sparql(
      `SELECT DISTINCT ?person ?personLabel ?role WHERE { VALUES ?work { ${values} } `
      + `${clauses} SERVICE wikibase:label { bd:serviceParam wikibase:language "${lang},en". } }`);
    for (const b of rows) {
      const pq = qval(b.person);
      const name = b.personLabel ? b.personLabel.value : pq;
      const role = b.role ? b.role.value : "";
      if (!people.has(pq)) people.set(pq, { name, roles: new Set() });
      people.get(pq).roles.add(role);
    }
  }
  return people;
}

/* ===== Wikipedia フォールバック ===== */
async function getSitelinks(qid, langs) {
  const sitefilter = langs.map(l => l + "wiki").join("|");
  const url = apiURL(WD_API, { action: "wbgetentities", ids: qid,
    props: "sitelinks", sitefilter });
  const j = await getJSON(url);
  const sl = (((j.entities || {})[qid]) || {}).sitelinks || {};
  const out = {};
  for (const l of langs) if (sl[l + "wiki"]) out[l] = sl[l + "wiki"].title;
  return out;
}

async function getWikitext(title, wikiLang) {
  const url = apiURL(`https://${wikiLang}.wikipedia.org/w/api.php`, {
    action: "query", prop: "revisions", rvprop: "content", rvslots: "main",
    titles: title, redirects: 1, formatversion: 2 });
  const j = await getJSON(url);
  const pages = (j.query && j.query.pages) || [];
  if (!pages.length || pages[0].missing) return null;
  try { return pages[0].revisions[0].slots.main.content; } catch (e) { return null; }
}

function cleanLinkTarget(raw, cfg) {
  let t = raw.split("|")[0].split("#")[0].trim();
  if (!t || cfg.ns.test(t)) return null;
  if (t.startsWith("http") || t.startsWith("//")) return null;
  return t;
}
const isSub = (name, cfg) => cfg.subKw.some(k => name.includes(k));

function extractCastLinks(text, cfg) {
  const links = new Set(), subs = new Set(), dubSecLinks = new Set();
  let inField = false;
  for (const line of text.split("\n")) {
    if (cfg.infobox.test(line)) inField = true;
    else if (inField && /^\s*(\||\}\})/.test(line)) inField = false;
    if (inField) for (const m of line.matchAll(WIKILINK)) {
      const t = cleanLinkTarget(m[1], cfg); if (t) links.add(t);
    }
  }
  const sections = text.split(/^(={2,}\s*.+?\s*={2,})\s*$/m);
  // キャスト見出しの配下サブセクションも対象にするため、見出しレベルを継承する
  let relLevel = 0, dubLevel = 0;
  for (let i = 1; i < sections.length; i += 2) {
    const heading = sections[i], body = sections[i + 1] || "";
    const level = (heading.match(/^=+/) || ["="])[0].length;
    // 同レベル以上の見出しに来たら、それまでのキャスト/吹替ブロックを抜ける
    if (relLevel && level <= relLevel) relLevel = 0;
    if (dubLevel && level <= dubLevel) dubLevel = 0;
    if (cfg.section.test(heading)) relLevel = level;
    if (cfg.dubSection && cfg.dubSection.test(heading)) dubLevel = level;
    const isDub = dubLevel > 0;                 // 吹替セクション（配下も含む）
    const relevant = relLevel > 0 || isDub;     // キャスト関連（配下も含む）
    const target = relevant ? body : heading + body;
    for (const m of target.matchAll(MAIN_TMPL)) {
      const name = m[1].split("#")[0].trim();
      if (isSub(name, cfg)) subs.add(name);
    }
    for (const m of target.matchAll(WIKILINK)) {
      const t = cleanLinkTarget(m[1], cfg); if (!t) continue;
      if (isSub(t, cfg)) subs.add(t);
      else if (isDub) dubSecLinks.add(t);   // 吹替表の人物は声優候補へ
      else if (relevant) links.add(t);
    }
  }
  return { links, subs, dubSecLinks };
}

// マーカー（声/CV や 吹替 など）の直後に並ぶwikilinkを抽出する汎用関数
function extractMarkedLinks(text, re, cfg) {
  const links = new Set();
  if (!re) return links;
  for (const m of text.matchAll(re))
    for (const w of m[1].matchAll(WIKILINK)) {
      const t = cleanLinkTarget(w[1], cfg); if (t) links.add(t);
    }
  return links;
}

async function filterHumans(titles, wikiLang, labelLang) {
  titles = [...new Set(titles.filter(Boolean))];
  if (!titles.length) return new Map();
  const domain = `https://${wikiLang}.wikipedia.org/`;
  const people = new Map();
  for (let i = 0; i < titles.length; i += 100) {
    const values = titles.slice(i, i + 100)
      .map(t => `<${domain}wiki/${encodeURIComponent(t.replace(/ /g, "_"))}>`).join(" ");
    const rows = await sparql(
      `SELECT DISTINCT ?person ?personLabel WHERE { VALUES ?article { ${values} } `
      + `?article schema:about ?person ; schema:isPartOf <${domain}> . `
      + `?person wdt:P31 wd:Q5 . `
      + `SERVICE wikibase:label { bd:serviceParam wikibase:language "${labelLang},en". } }`);
    for (const b of rows) people.set(qval(b.person), b.personLabel ? b.personLabel.value : qval(b.person));
  }
  return people;
}

// 本文のキャスト欄＝「出演」、声/CVマーカーや日本語吹替＝「声優」に振り分けて抽出。
// 吹替声優は出演から除外する（俳優ではなく日本語版の声優なので）。
async function getPeopleWikipedia(title, wikiLang, labelLang) {
  const cfg = WIKI_CONFIG[wikiLang]; if (!cfg) return new Map();
  const text = await getWikitext(title, wikiLang); if (!text) return new Map();
  const { links: castLinks, subs, dubSecLinks } = extractCastLinks(text, cfg);
  const voiceLinks = extractMarkedLinks(text, cfg.va, cfg);     // 声/CV由来
  for (const sub of [...subs].slice(0, 5)) {
    const st = await getWikitext(sub, wikiLang);
    if (st) for (const x of extractMarkedLinks(st, cfg.va, cfg)) voiceLinks.add(x);
  }
  const dubLinks = extractMarkedLinks(text, cfg.dub, cfg);      // インライン吹替由来
  // インラインの声優/吹替は出演から除外（キャスト欄に紛れていても声優として扱う）
  for (const t of voiceLinks) castLinks.delete(t);
  for (const t of dubLinks) castLinks.delete(t);

  const out = new Map();
  const cast = await filterHumans([...castLinks], wikiLang, labelLang);
  for (const [pq, name] of cast) out.set(pq, { name, roles: new Set(["出演"]) });
  // 声/CV・インライン吹替・吹替セクション（表）を声優として統合
  //（吹替セクションは俳優も混ざるが、俳優はキャスト節の出演を残したまま声優も付く）
  const voiceAll = [...new Set([...voiceLinks, ...dubLinks, ...dubSecLinks])];
  const voice = await filterHumans(voiceAll, wikiLang, labelLang);
  for (const [pq, name] of voice) {
    if (out.has(pq)) out.get(pq).roles.add("声優");
    else out.set(pq, { name, roles: new Set(["声優"]) });
  }
  return out;
}

/* ===== 統合 ===== */
function mergePeople(dst, src) {
  for (const [pq, val] of src) {
    if (dst.has(pq)) for (const r of val.roles) dst.get(pq).roles.add(r);
    else dst.set(pq, { name: val.name, roles: new Set(val.roles) });
  }
}

async function collectPeople(qid, title, lang, wikiLangs, alwaysWiki, props, onStatus) {
  onStatus(`「${title}」Wikidata取得中`);
  const people = await getPeopleWikidata(qid, lang, props, true);
  // 取得元は {text, url} で持つ（表示時にリンク化）
  const sources = [{ text: "Wikidata", url: `https://www.wikidata.org/wiki/${qid}` }];
  if (wikiLangs.length && (alwaysWiki || people.size < WEAK_THRESHOLD)) {
    const sites = await getSitelinks(qid, wikiLangs);
    for (const wl of wikiLangs) {
      const wt = sites[wl] || (wl === lang ? title : null);
      if (!wt) continue;
      onStatus(`「${title}」Wikipedia(${wl})解析中`);
      const found = await getPeopleWikipedia(wt, wl, lang);
      mergePeople(people, found);
      if (found.size) sources.push({
        text: `Wikipedia(${wl})`,
        note: `:${found.size}人`,
        url: `https://${wl}.wikipedia.org/wiki/${encodeURIComponent(wt.replace(/ /g, "_"))}`,
      });
    }
  }
  return { people, sources };
}

async function compare(titles, roleKeys, wikiLangs, lazy, lang, onStatus) {
  if (roleKeys.includes("all")) roleKeys = Object.keys(ROLE_PROPS);
  let props = [];
  for (const r of roleKeys)
    for (const pl of (ROLE_PROPS[r] || []))
      if (!props.some(x => x[0] === pl[0])) props.push(pl);
  if (!props.length) props = ROLE_PROPS.cast;
  wikiLangs = wikiLangs.filter(l => WIKI_CONFIG[l]);
  const alwaysWiki = !lazy;

  const works = [];
  for (const v0 of titles) {
    const value = v0.trim();
    let info;
    if (/^Q\d+$/i.test(value)) {
      const qid = value.toUpperCase();
      onStatus(`${qid} を取得中`);
      const ei = await getEntityInfo(qid, lang);   // QID入力でも日本語名を表示
      info = { qid, label: ei.label, description: ei.description };
    } else { onStatus(`「${value}」を検索中`); info = await resolveQid(value, lang); }
    if (!info || !info.qid) throw new Error("作品が見つかりません: " + value);
    info.input = value;
    works.push(info);
  }

  const collected = [];
  for (const w of works)
    collected.push(await collectPeople(w.qid, w.label, lang, wikiLangs, alwaysWiki, props, onStatus));

  const peoples = collected.map(c => c.people);
  let commonIds = [...peoples[0].keys()];
  for (let i = 1; i < peoples.length; i++) commonIds = commonIds.filter(id => peoples[i].has(id));
  commonIds.sort((a, b) => peoples[0].get(a).name.localeCompare(peoples[0].get(b).name, "ja"));

  return {
    works: works.map((w, i) => Object.assign({}, w, { count: peoples[i].size, sources: collected[i].sources })),
    common: commonIds.map(pq => ({
      name: peoples[0].get(pq).name, qid: pq,
      roles: peoples.map(p => [...p.get(pq).roles].sort()),
    })),
  };
}

/* ===== UI ===== */
const ROLE_LABELS = {};
for (const [k, v] of Object.entries(ROLE_PROPS)) ROLE_LABELS[k] = v.map(x => x[1]).join("/");

const worksEl = document.getElementById("works");
const rolesEl = document.getElementById("roles");
const wlangsEl = document.getElementById("wlangs");

function addWork(value = "") {
  const row = document.createElement("div"); row.className = "work-row";
  const inp = document.createElement("input"); inp.type = "text"; inp.value = value;
  inp.placeholder = "例: SPL2、まどマギ、Avengers、Q16923905（略称・英題でもOK）";
  inp.addEventListener("keydown", e => { if (e.key === "Enter") run(); });
  const up = document.createElement("button");
  up.className = "icon"; up.textContent = "↑"; up.title = "上へ";
  up.onclick = () => { const p = row.previousElementSibling; if (p) worksEl.insertBefore(row, p); };
  const down = document.createElement("button");
  down.className = "icon"; down.textContent = "↓"; down.title = "下へ";
  down.onclick = () => { const n = row.nextElementSibling; if (n) worksEl.insertBefore(n, row); };
  const del = document.createElement("button");
  del.className = "icon"; del.textContent = "×"; del.title = "削除";
  del.onclick = () => { if (worksEl.children.length > 2) row.remove(); };
  row.append(inp, up, down, del); worksEl.append(row);
}
addWork(); addWork();

for (const [key, label] of Object.entries(ROLE_LABELS)) {
  const s = document.createElement("span"); s.className = "chk";
  s.innerHTML = `<input type="checkbox" class="role" value="${key}" `
    + (DEFAULT_ROLES.includes(key) ? "checked" : "") + `>${label}`;
  rolesEl.append(s);
}

for (const [code, label] of Object.entries(LANG_LABELS)) {
  const s = document.createElement("span"); s.className = "chk";
  s.innerHTML = `<input type="checkbox" class="wl" value="${code}" `
    + (DEFAULT_LANGS.includes(code) ? "checked" : "") + `>${label}`;
  wlangsEl.append(s);
}

const langPresetsEl = document.getElementById("langPresets");
for (const [label, codes] of Object.entries(LANG_PRESETS)) {
  const b = document.createElement("button");
  b.className = "mini"; b.type = "button"; b.textContent = label;
  b.title = codes.map(c => LANG_LABELS[c] || c).join(" / ");
  b.onclick = () => setDefault(".wl", codes);
  langPresetsEl.append(b);
}

const getChecked = sel => [...document.querySelectorAll(sel)].filter(c => c.checked).map(c => c.value);
const setAll = (sel, v) => document.querySelectorAll(sel).forEach(c => { c.checked = v; });
const setDefault = (sel, defaults) =>
  document.querySelectorAll(sel).forEach(c => { c.checked = defaults.includes(c.value); });

function setStatus(html, warn = false) {
  const s = document.getElementById("status"); s.innerHTML = html; s.className = warn ? "warn" : "";
}
const esc = s => (s || "").replace(/[&<>]/g, c => ({ "&": "&amp;", "<": "&lt;", ">": "&gt;" }[c]));

async function run() {
  const titles = [...worksEl.querySelectorAll("input")].map(i => i.value.trim()).filter(Boolean);
  if (titles.length < 2) { setStatus("作品を2つ以上入力してください", true); return; }
  const roles = getChecked(".role"); if (!roles.length) roles.push("cast");
  setStatus('<span class="spin"></span>準備中…');
  document.getElementById("run").disabled = true;
  document.getElementById("result").innerHTML = "";
  try {
    const data = await compare(titles, roles, getChecked(".wl"),
      document.getElementById("lazy").checked, "ja",
      msg => setStatus('<span class="spin"></span>' + esc(msg) + "…"));
    setStatus(""); render(data);
  } catch (e) { setStatus("エラー: " + (e.message || e), true); }
  document.getElementById("run").disabled = false;
}

function render(data) {
  let h = '<div class="card"><label class="fld">対象作品</label><table><tr>'
        + "<th>#</th><th>作品</th><th>取得元</th></tr>";
  data.works.forEach((w, i) => {
    let note = "";
    if (w.matched_query && w.matched_query !== w.input)
      note += `<div class="warn">ℹ「${esc(w.input)}」→「${esc(w.matched_query)}」で再検索</div>`;
    if (w.is_work === false)
      note += `<div class="warn">⚠ 作品として認識できず別物の可能性</div>`;
    const srcHtml = w.sources.map(s => {
      const link = s.url
        ? `<a href="${s.url}" target="_blank">${esc(s.text)}</a>`
        : esc(s.text);
      return link + (s.note ? esc(s.note) : "");   // 「:n人」はリンク外
    }).join(", ");
    h += `<tr><td>${i + 1}</td><td>${esc(w.label)} `
       + `<a class="qid" href="https://www.wikidata.org/wiki/${w.qid}" target="_blank">${w.qid}</a>`
       + `<div class="muted">${esc(w.description || "")}</div>${note}</td>`
       + `<td class="muted">${srcHtml}<br>${w.count}人</td></tr>`;
  });
  h += "</table></div>";

  if (!data.common.length) {
    h += `<div class="card">${data.works.length}作品すべてに共通する人物は見つかりませんでした。</div>`;
  } else {
    h += `<div class="card"><label class="fld">${data.works.length}作品すべてに共通：`
       + `${data.common.length}人</label><table><tr><th>名前</th>`;
    data.works.forEach((w, i) => h += `<th>作品${i + 1}</th>`);
    h += "</tr>";
    data.common.forEach(p => {
      h += `<tr><td>${esc(p.name)} `
         + `<a class="qid" href="https://www.wikidata.org/wiki/${p.qid}" target="_blank">${p.qid}</a></td>`;
      p.roles.forEach(rs => { h += "<td>" + rs.map(r => `<span class="pill">${esc(r)}</span>`).join("") + "</td>"; });
      h += "</tr>";
    });
    h += "</table></div>";
  }
  document.getElementById("result").innerHTML = h;
}

document.getElementById("run").onclick = run;
document.getElementById("addWork").onclick = () => addWork();
document.getElementById("roleAll").onclick = () => setAll(".role", true);
document.getElementById("roleNone").onclick = () => setAll(".role", false);
document.getElementById("roleDef").onclick = () => setDefault(".role", DEFAULT_ROLES);
document.getElementById("langAll").onclick = () => setAll(".wl", true);
document.getElementById("langNone").onclick = () => setAll(".wl", false);

/* ===== テーマ（ライト/ダーク）===== */
const themeBtn = document.getElementById("themeBtn");
function applyTheme(t) {
  document.documentElement.dataset.theme = t;
  themeBtn.textContent = t === "light" ? "🌙 ダーク" : "☀ ライト";
  try { localStorage.setItem("cafTheme", t); } catch (e) { /* file:// 等 */ }
}
let savedTheme = null;
try { savedTheme = localStorage.getItem("cafTheme"); } catch (e) {}
if (!savedTheme)
  savedTheme = matchMedia("(prefers-color-scheme: light)").matches ? "light" : "dark";
applyTheme(savedTheme);
themeBtn.onclick = () =>
  applyTheme(document.documentElement.dataset.theme === "light" ? "dark" : "light");
