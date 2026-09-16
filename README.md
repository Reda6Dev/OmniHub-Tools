# 🧩 OmniHub Official Tools Catalog

> **مستودع الأدوات الرسمية لمنصة OmniHub** — منصة أتمتة مفتوحة المصدر مبنية بلغة C# وإطار عمل WPF، تعتمد بالكامل على مبدأ التكوين عبر ملفات JSON (Config-Driven Architecture).

هذا المستودع هو **الكتالوج السحابي الرسمي** لأدوات منصة OmniHub، ويحتوي على مجموعة متكاملة من 50 أداة احترافية جاهزة للاستخدام الفوري، موزعة على أربعة أجنحة تخصصية تغطي تطوير الدوت نت وGit، ومحرك Godot، وصيانة الألعاب والمودات، ومعالجة الوسائط والأصول.

---

## 📖 جدول المحتويات

- [نظرة عامة](#-نظرة-عامة)
- [هيكلية المستودع](#-هيكلية-المستودع)
- [فهرس الأدوات الكامل (50 أداة)](#-فهرس-الأدوات-الكامل-50-أداة)
  - [📦 جناح .NET \& Git (13 أداة)](#-جناح-net--git-13-أداة)
  - [🎮 جناح Godot Engine (12 أداة)](#-جناح-godot-engine-12-أداة)
  - [🛠️ جناح صيانة الألعاب والمودات (13 أداة)](#️-جناح-صيانة-الألعاب-والمودات-13-أداة)
  - [🎨 جناح معالجة الوسائط والأصول (12 أداة)](#-جناح-معالجة-الوسائط-والأصول-12-أداة)
- [طريقة التثبيت والاستخدام داخل OmniHub](#-طريقة-التثبيت-والاستخدام-داخل-omnihub)
- [كيفية إضافة أداة JSON جديدة](#-كيفية-إضافة-أداة-json-جديدة)
- [الترخيص والأمان](#-الترخيص-والأمان)

---

## 🔎 نظرة عامة

منصة **OmniHub** هي منصة أتمتة سطح مكتب مفتوحة المصدر مصممة لمطوري الألعاب والمبرمجين ومنشئي المودات، تسمح بتشغيل مهام تقنية متكررة (بناء، تنظيف، تصدير، ضغط وسائط، صيانة Git...) بضغطة زر واحدة، دون الحاجة لكتابة سكربتات يدوية في كل مرة.

كل أداة في هذا الكتالوج هي في جوهرها **ملف تكوين JSON واحد** يصف:

- **البيانات الوصفية** للأداة (الاسم، الوصف، الأيقونة، الجناح/التصنيف) بالعربية والإنجليزية.
- **طريقة التنفيذ** (`execution`) عبر نمط `ExternalProcess`، والذي يشغّل أوامر PowerShell أو أوامر نظام مباشرة (`git`, `dotnet`, `ffmpeg`, `magick`, `godot`... إلخ) بشكل آمن ومتحكم به.
- **المعاملات** (`parameters`) التي تحدد المدخلات المطلوبة من المستخدم (مسار ملف، مسار مجلد، قائمة منسدلة، نص حر، قيمة منطقية) مع قيم افتراضية ذكية لتسريع الاستخدام.

بما أن المنصة بالكامل مبنية على مبدأ Config-Driven، فإن إضافة أداة جديدة لا تتطلب أي تعديل على كود C# الأساسي للمنصة؛ يكفي إضافة ملف JSON جديد مطابق للمخطط القياسي.

---

## 🗂️ هيكلية المستودع

```
OmniHub-Tools/
│
├── catalog.json              # ملف الفهرس السحابي المركزي (يحتوي بيانات كل الأدوات الـ 50)
│
├── configs/
│   └── tools/                 # كل أداة محفوظة في ملف JSON مستقل باسم معرفها (id)
│       ├── dotnet.singlefile.publish.json
│       ├── git.quick.commit.sync.json
│       ├── godot.headless.export.json
│       ├── game.hung.process.killer.json
│       ├── media.ico.generator.json
│       └── ...                # (50 ملف أداة بالإجمالي)
│
└── README.md                  # هذا الملف
```

### 📌 دور `catalog.json`

يحتوي هذا الملف على مصفوفة `tools`، وكل عنصر بداخلها يمثّل **بطاقة عرض مختصرة** لأداة معينة تستخدمها واجهة متجر الأدوات (Tool Store) داخل تطبيق OmniHub لعرض قائمة الأدوات المتاحة للتنزيل، ويتضمن كل عنصر:

| الحقل | الوصف |
|---|---|
| `id` | المعرف الفريد للأداة، ويُستخدم كاسم لملف الأداة الفعلي. |
| `name` / `nameAr` | اسم الأداة بالإنجليزية والعربية. |
| `description` / `descriptionAr` | وصف مختصر من سطر واحد للعرض في بطاقة المتجر. |
| `version` | رقم إصدار الأداة، ويبدأ عادة بـ `1.0.0`. |
| `author` | ناشر الأداة (`Reda6Dev`). |
| `category` | اسم الجناح الذي تنتمي إليه الأداة. |
| `downloadUrl` | رابط GitHub الخام (Raw) لتنزيل ملف تكوين الأداة الكامل مباشرة. |

### 📌 دور `configs/tools/{id}.json`

كل ملف داخل هذا المجلد هو **ملف التكوين الكامل والفعلي** للأداة، ويحتوي إضافة إلى البيانات الوصفية السابقة على:

- كائن `execution`: يحدد نوع التنفيذ (`ExternalProcess` دوماً في هذا الكتالوج)، والبرنامج التنفيذي (`executable`)، وقالب المعاملات (`argumentsTemplate`) الذي يستخدم صيغة `${ParameterId}` لتمرير قيم المدخلات، ومجلد العمل الاختياري (`workingDirectory`).
- مصفوفة `parameters`: تصف كل حقل إدخال يحتاجه المستخدم (`id`, `label`, `type`, `required`, `defaultValue`, و`options` في حال كان النوع `Dropdown`).

---

## 📋 فهرس الأدوات الكامل (50 أداة)

### 📦 جناح .NET & Git (13 أداة)

يغطي هذا الجناح مهام تطوير مشاريع C#/.NET وبايثون، وإدارة مستودعات Git، من نشر وتحديث وتنظيف حتى مزامنة سريعة وقياس جودة الكود.

| # | الاسم (EN) | الاسم (AR) | المعرف التقني (ID) | الوظيفة الفنية |
|---|---|---|---|---|
| 1 | Single-File Publisher | ناشر الملف التنفيذي الواحد | `dotnet.singlefile.publish` | تشغيل أمر dotnet publish مع خيارات PublishSingleFile=true و SelfContained=true |
| 2 | NuGet Global Cache Nuker | مُفرِّغ كاش نيوجت الشامل | `dotnet.nuget.cache.nuker` | تنفيذ الأمر dotnet nuget locals all --clear الذي يحذف جميع أنواع الكاش المحلية الخاصة بحزم NuGet |
| 3 | Bin & Obj Deep Cleaner | منظّف مجلدات bin و obj العميق | `dotnet.clean.binobj` | البحث بشكل تكراري (Recursive) داخل المجلد الجذري المحدد عن كل مجلدات bin و obj الخاصة بمشاريع C# |
| 4 | Git Quick Commit & Sync | مزامنة واعتماد سريع لتعديلات Git | `git.quick.commit.sync` | تنفيذ تسلسل أوامر Git متكامل: git pull لجلب آخر التحديثات من المستودع البعيد |
| 5 | Git Discard & Clean | إلغاء التعديلات والتنظيف الكامل | `git.discard.clean` | تنفيذ الأمرين git reset --hard و git clean -fd معاً |
| 6 | Python Venv Creator & Setup | مُنشئ ومجهّز بيئات بايثون | `python.venv.setup` | إنشاء بيئة عمل افتراضية (Virtual Environment) داخل مجلد المشروع المحدد باستخدام python -m venv |
| 7 | Python Bulk Pip Packages Upgrader | مُحدّث مكتبات بايثون الشامل | `python.pip.upgrade.all` | جلب قائمة كل حزم بايثون المثبتة والتي لديها إصدار أحدث متاح باستخدام pip list --outdated |
| 8 | Git Merged Branches Purger | منظف فروع Git المدمجة | `git.merged.branches.cleaner` | فحص كل الفروع المحلية (Local Branches) داخل المستودع المحدد باستخدام git branch --merged |
| 9 | .NET Outdated Packages Reporter | كاشف حزم دوت نت القديمة | `dotnet.outdated.packages` | تشغيل الأمر dotnet list package --outdated على المشروع أو ملف الحل المحدد |
| 10 | Git Stash & Pop Manager | مدير حفظ واسترجاع تعديلات Git | `git.stash.pop.manager` | تنفيذ أمر Git Stash المختار من القائمة المنسدلة داخل المستودع المحدد؛ فخيار stash push -u يقوم بحفظ كل التعديلات الحالية غير المعتمدة (بما فيها الملفات غير المتتبعة) جانباً في مخزن مؤقت وإعادة الفرع إلى حالته النظيفة |
| 11 | Git Large Objects & Size Analyzer | فاحص كائنات وحجم مستودع Git | `git.repo.size.analyzer` | تشغيل أمر git verify-pack -v على ملف الحزمة (Pack File) الرئيسي داخل مجلد .git للمستودع المحدد |
| 12 | C# Lines of Code Counter | حاسب أسطر كود مشاريع C# | `csharp.code.metrics` | المرور بشكل تكراري على كل ملفات C# ذات الامتداد .cs داخل مجلد المشروع المحدد |
| 13 | Python PEP8 Auto-Formatter | منسق سكربتات بايثون الآلي | `python.format.autopep8` | تشغيل أداة autopep8 على ملف سكربت بايثون المحدد باستخدام الخيارين --in-place لحفظ التعديلات مباشرة داخل نفس الملف |

### 🎮 جناح Godot Engine (12 أداة)

يغطي هذا الجناح كل ما يتعلق بمحرك الألعاب Godot: التصدير التلقائي بدون واجهة، تنظيف الكاش، فحص السكربتات والشيدرات، والنسخ الاحتياطي قبل الترقية.

| # | الاسم (EN) | الاسم (AR) | المعرف التقني (ID) | الوظيفة الفنية |
|---|---|---|---|---|
| 1 | Godot Headless Multi-Platform Exporter | مُصدِّر جودو التلقائي متعدد المنصات | `godot.headless.export` | تشغيل محرك جودو بوضع Headless (بدون واجهة رسومية) باستخدام المعامل --export-release |
| 2 | Godot Deep Cache & Locks Nuker | مُفرِّغ كاش جودو وملفات الأقفال | `godot.cache.nuker` | حذف المجلد المخفي .godot الذي يحتوي على ملفات الاستيراد المؤقتة (Import Cache) |
| 3 | GDScript Static Code Checker | فاحص أخطاء GDScript الساكن | `godot.gdscript.check` | تشغيل محرك جودو بوضع Headless مع المعامل --check-only |
| 4 | Godot Project Version Migrator | أداة النسخ الاحتياطي قبل ترقية إصدار جودو | `godot.project.backup` | أخذ نسخة احتياطية كاملة ومضغوطة (ZIP) لمجلد مشروع جودو بالكامل |
| 5 | Godot PCK Mod Packer | حازم ومستخرج حزم PCK لجودو | `godot.pck.packer` | تجميع مجلد أصول داخل ملف PCK أو استخراج ملف PCK موجود إلى مجلد، عبر Godot CLI Headless |
| 6 | Godot Orphaned .import Cleaner | منظف مستوردات جودو اليتيمة | `godot.texture.cleaner` | المرور بشكل تكراري على كل ملفات .import الموجودة داخل مجلد المشروع |
| 7 | GDScript Documentation Generator | مولد توثيق سكربتات جودو | `godot.gdscript.docgen` | فحص كل ملفات GDScript ذات الامتداد .gd داخل مجلد المشروع بشكل تكراري |
| 8 | Godot C# Headless Project Builder | باني مشاريع جودو C# بدون واجهة | `godot.mono.build` | تشغيل أمر dotnet build مباشرة على ملف الحل الخاص بمشروع جودو الذي يستخدم لغة C# (Godot Mono) |
| 9 | Godot Shader Syntax Validator | فاحص صحة ملفات شيدر جودو | `godot.shader.cleaner` | المرور بشكل تكراري على كل ملفات الشيدر ذات الامتداد .gdshader داخل مجلد المشروع |
| 10 | Godot Unused Assets Finder | كاشف أصول جودو غير المستخدمة | `godot.find.unused.assets` | جمع قائمة بكل ملفات الأصول (الصور والأصوات والنماذج) الموجودة داخل مجلد المشروع |
| 11 | Godot Old Export Templates Purger | منظف قوالب تصدير جودو القديمة | `godot.export.templates.cleaner` | فحص مسار قوالب التصدير الخاص بمحرك جودو %APPDATA%\Godot\export_templates |
| 12 | Godot 4 UID References Validator | فاحص مراجع UID لمشاريع جودو | `godot.project.uid.repair` | فحص كل ملفات .uid وملفات .import الموجودة داخل مشروع جودو 4 |

### 🛠️ جناح صيانة الألعاب والمودات (13 أداة)

يغطي هذا الجناح أدوات صيانة النظام والألعاب المثبتة: إنهاء العمليات المعلقة، فك حظر المودات، إدارة الجدار الناري والمنافذ، والنسخ الاحتياطي لملفات الحفظ.

| # | الاسم (EN) | الاسم (AR) | المعرف التقني (ID) | الوظيفة الفنية |
|---|---|---|---|---|
| 1 | Game & Hung Process Instant Killer | منهي العمليات المعلقة الفوري | `game.hung.process.killer` | تنفيذ أمر إنهاء قسري (taskkill /F) على اسم العملية المحدد |
| 2 | Downloaded Mod & DLL Unblocker | فاك حظر مودات وملفات DLL المنزّلة | `game.mod.unblocker` | المرور على كافة الملفات الموجودة داخل مجلد المودات المحدد بشكل تكراري (Recursive) |
| 3 | Crash Dumps & Temp Purger | مُفرِّغ ملفات الانهيار المؤقتة | `system.crash.dumps.cleaner` | البحث عن كل الملفات ذات الامتداد .dmp داخل المجلد المحدد (والذي يكون افتراضياً مجلد تفريغ الانهيارات الخاص بويندوز %LOCALAPPDATA%\CrashDumps) وحذفها بالكامل |
| 4 | TCP/Port Liberator | محرر المنافذ المحتجزة | `system.port.killer` | البحث عن العملية (Process) التي تحتجز رقم المنفذ (Port) المحدد باستخدام أمر Get-NetTCPConnection |
| 5 | XML Mod Localization Diff Checker | فاحص فروقات تعريب ملفات XML | `game.xml.localization.diff` | مقارنة ملف الترجمة الأصلي (الإنجليزي عادةً) مع ملف الترجمة المستهدف كلاهما بصيغة XML |
| 6 | Game Mods Directory Junction Maker | صانع الروابط الصلبة لمجلدات المودات | `game.junction.symlink` | تنفيذ أمر mklink /J لإنشاء وصلة مجلد صلبة (Directory Junction) بين مسار وهمي داخل مجلد اللعبة (LinkPath) ومجلد فعلي موجود على قرص آخر أو مسار مختلف (TargetPath) |
| 7 | Bannerlord SubModule.xml Validator | فاحص ملفات تعريف مودات بانرلورد | `bannerlord.submodule.validator` | قراءة وتحليل ملف SubModule.xml الموجود داخل مجلد مود لعبة Mount & Blade II: Bannerlord |
| 8 | Game Saves Timestamped Auto-Backup | النسخ الاحتياطي التلقائي والمؤرخ لحفظ الألعاب | `game.saves.backup` | ضغط كامل محتويات مجلد حفظ اللعبة المحدد (SavesDir) داخل ملف أرشيف ZIP واحد |
| 9 | Network DNS Flush & Winsock Reset | مفرغ كاش DNS ومصلح اتصال الألعاب | `system.network.flush.repair` | تنفيذ أمرين متتاليين لإصلاح مشاكل الاتصال الشائعة في الألعاب الجماعية عبر الإنترنت: الأمر الأول ipconfig /flushdns يقوم بتفريغ ذاكرة التخزين المؤقت لأسماء النطاقات (DNS Cache) |
| 10 | Bulk Archives Multi-Folder Extractor | المفكك الجماعي للملفات المضغوطة | `archive.bulk.extractor` | البحث عن كل ملفات الأرشيف بصيغة ZIP الموجودة مباشرة داخل المجلد المحدد |
| 11 | Game Executable Firewall Permitter | مُفوِّض ألعاب جدار حماية ويندوز | `game.firewall.rule.manager` | تنفيذ أمر netsh advfirewall لإضافة قاعدة سماح جديدة (Allow Rule) في جدار حماية ويندوز للاتصالات الواردة (Inbound) الخاصة بالملف التنفيذي المحدد |
| 12 | Game Config Resolution Overrider | مصلح دقة شاشة الألعاب | `game.resolution.config.fixer` | تقوم هذه الأداة أولاً بأخذ نسخة احتياطية فورية من ملف الإعدادات المدخل بإضافة لاحقة .bak قبل أي تعديل |
| 13 | Empty Directories Purger | منظف المجلدات الفارغة الشامل | `system.empty.folders.nuker` | الفحص التكراري (Recursive) لكل المجلدات الفرعية الموجودة داخل المسار المحدد |

### 🎨 جناح معالجة الوسائط والأصول (12 أداة)

يغطي هذا الجناح تحويل وضغط ومعالجة الصور والصوتيات والفيديوهات وملفات النصوص المستخدمة كأصول داخل الألعاب.

| # | الاسم (EN) | الاسم (AR) | المعرف التقني (ID) | الوظيفة الفنية |
|---|---|---|---|---|
| 1 | Multi-Size Windows ICO Generator | مولّد أيقونات ويندوز متعددة المقاسات | `media.ico.generator` | استخدام محرك ImageMagick لتحويل صورة PNG أو JPG المدخلة إلى ملف أيقونة ويندوز (.ico) يحتوي على عدة مقاسات مدمجة داخل الملف الواحد (16x16, 32x32, 48x48, 256x256) |
| 2 | Game Audio to OGG Vorbis Converter | محوّل الصوتيات إلى صيغة OGG Vorbis | `media.audio.to_ogg` | استخدام محرك FFmpeg لتحويل ملف الصوت المدخل إلى صيغة OGG Vorbis المضغوطة |
| 3 | Video Compressor H.264 | ضاغط الفيديو بصيغة H.264 | `media.video.compress.h264` | استخدام محرك FFmpeg لإعادة ترميز ملف الفيديو المدخل باستخدام مرمّز H.264 (libx264) وإخراجه بصيغة MP4 |
| 4 | Image WebP Converter | محوّل الصور إلى صيغة WebP | `media.image.to_webp` | استخدام محرك ImageMagick لتحويل الصورة المدخلة إلى صيغة WebP الحديثة التي توفر نسبة ضغط عالية جداً مقارنة بصيغ PNG و JPG التقليدية مع الحفاظ على جودة بصرية مقبولة |
| 5 | Text UTF-8 BOM-Free Normalizer | مُوحِّد ترميز النصوص UTF-8 بدون BOM | `media.text.utf8.normalizer` | قراءة محتوى ملف النص أو الترجمة المدخل مهما كان ترميزه الأصلي |
| 6 | Animated GIF to Lightweight MP4 | محول صور GIF إلى فيديو MP4 فائق الخفة | `media.gif.to.mp4` | استخدام محرك FFmpeg لتحويل ملف الصورة المتحركة GIF المدخل إلى فيديو بصيغة MP4 |
| 7 | Bulk Translation Text Replacer | مستبدل مصطلحات التعريب المجمع | `text.bulk.regex.replacer` | المرور على كل الملفات المطابقة لنمط الفلتر المحدد (مثل *.xml أو *.json) داخل المجلد الهدف بشكل تكراري |
| 8 | Game Audio Loudness Normalizer (EBU R128) | موحد وموازن علو الصوتيات للألعاب | `media.audio.normalize` | تشغيل فلتر loudnorm الخاص بمحرك FFmpeg والمعتمد على معيار EBU R128 العالمي لقياس وتوحيد مستوى علو الصوت (Loudness) في ملف الصوت المدخل |
| 9 | Video Audio Stream Extractor | مستخرج الصوت من مقاطع الفيديو | `media.video.extract.audio` | استخدام محرك FFmpeg لاستخراج مسار الصوت (Audio Stream) الموجود داخل ملف الفيديو المدخل بشكل منفصل عن الصورة |
| 10 | JSON Beautifier & Indentation Fixer | منسق ومصلح ملفات JSON | `text.json.formatter` | قراءة محتوى ملف JSON المحدد وتحليله بنيوياً (Parsing) للتأكد من صحة صياغته |
| 11 | Bulk Image Resizer | مغيّر أبعاد الصور الجماعي | `media.image.resize.bulk` | المرور على كل ملفات الصور الموجودة داخل مجلد الإدخال المحدد |
| 12 | Audio Auto Silence Trimmer | قاطع الصمت التلقائي للصوتيات | `media.audio.trim.silence` | استخدام محرك FFmpeg وفلتر silenceremove مرتين متتاليتين (مع عكس اتجاه الملف areverse بينهما) لاكتشاف وقص أي فترات صمت زائدة موجودة في بداية ونهاية ملف الصوت المدخل |

---

## ⚙️ طريقة التثبيت والاستخدام داخل OmniHub

### 1. التثبيت عبر متجر الأدوات المدمج (الطريقة الموصى بها)

1. افتح تطبيق **OmniHub** على جهازك.
2. انتقل إلى تبويب **متجر الأدوات (Tool Store)** من القائمة الجانبية.
3. تأكد من أن رابط الكتالوج مضبوط على:
   ```
   https://raw.githubusercontent.com/Reda6Dev/OmniHub-Tools/main/catalog.json
   ```
4. ستظهر لك قائمة الأدوات الـ 50 موزعة حسب أجنحتها؛ اضغط **تثبيت** بجانب أي أداة تريدها، وسيقوم التطبيق تلقائياً بتنزيل ملف الـ JSON الخاص بها من `downloadUrl` وإضافتها لواجهتك.

### 2. التثبيت اليدوي (Offline)

1. نزّل الملف المضغوط أو ملف JSON للأداة المطلوبة من مجلد `configs/tools/` في هذا المستودع.
2. افتح مجلد بيانات OmniHub المحلي على جهازك، عادة ما يكون في المسار:
   ```
   %APPDATA%\OmniHub\configs\tools\
   ```
3. انسخ ملف/ملفات JSON إلى هذا المجلد.
4. أعد تشغيل التطبيق أو اضغط زر **تحديث الأدوات (Refresh Tools)**؛ ستظهر الأداة الجديدة تلقائياً ضمن جناحها المناسب.

### 3. تشغيل أداة

1. اختر الأداة المطلوبة من الواجهة الرئيسية.
2. املأ الحقول المطلوبة (مسارات الملفات/المجلدات، القوائم المنسدلة، النصوص) — ستظهر القيم الافتراضية الذكية معبّأة مسبقاً حيثما أمكن.
3. اضغط زر **تشغيل (Run)**؛ سيقوم OmniHub بتعويض قيم المعاملات داخل `argumentsTemplate` وتشغيل الأمر عبر `ExternalProcess` وعرض سجل الإخراج مباشرة.

---

## ➕ كيفية إضافة أداة JSON جديدة

لإضافة أداة جديدة إلى الكتالوج، اتبع الخطوات التالية:

1. **أنشئ ملف JSON جديد** داخل `configs/tools/` باسم معرف الأداة، مثال: `my.custom.tool.json`.
2. **التزم بالمخطط القياسي** التالي:
   ```json
   {
     "id": "wing.tool.name",
     "name": "English Name",
     "nameAr": "الاسم بالعربية",
     "description": "Short English summary.",
     "descriptionAr": "شرح توضيحي دقيق بالعربية لوظيفة الأداة وتأثيرها على الملفات.",
     "category": "اسم الجناح المطابق",
     "execution": {
       "type": "ExternalProcess",
       "executable": "powershell.exe",
       "argumentsTemplate": "-NoProfile -ExecutionPolicy Bypass -Command \"...\"",
       "workingDirectory": null
     },
     "parameters": [
       {
         "id": "ParamId",
         "label": "عنوان الحقل",
         "type": "FilePath | DirectoryPath | Dropdown | Text | Boolean",
         "required": true,
         "defaultValue": "",
         "options": []
       }
     ]
   }
   ```
3. استخدم صيغة `${ParameterId}` حصراً داخل `argumentsTemplate` للإشارة إلى قيم المعاملات المدخلة من المستخدم.
4. **أضف عنصراً مطابقاً** داخل مصفوفة `tools` في `catalog.json` الرئيسي يحتوي على الحقول المختصرة (`id`, `name`, `nameAr`, `description`, `descriptionAr`, `version`, `author`, `category`, `downloadUrl`)، مع ضبط `downloadUrl` على:
   ```
   https://raw.githubusercontent.com/Reda6Dev/OmniHub-Tools/main/configs/tools/{id}.json
   ```
5. تأكد من أن معرف الأداة (`id`) **فريد** ولا يتكرر مع أي أداة موجودة مسبقاً في الكتالوج.
6. افتح **Pull Request** يتضمن ملف الأداة الجديد + التعديل على `catalog.json`، وسيتم مراجعته قبل الدمج.

---

## 🔐 الترخيص والأمان

### الترخيص

هذا المستودع وكل ملفات الأدوات بداخله متاحة كمشروع **مفتوح المصدر** بموجب رخصة **MIT License**. يمكن لأي شخص استخدام هذه الأدوات أو تعديلها أو إعادة توزيعها بحرية، مع الاحتفاظ بإشعار حقوق النشر الأصلي. راجع ملف `LICENSE` في جذر المستودع للتفاصيل الكاملة.

### ملاحظات أمنية هامة

> ⚠️ **تحذير:** تعمل جميع الأدوات في هذا الكتالوج عبر تنفيذ أوامر نظام حقيقية (`ExternalProcess`) مثل PowerShell و Git و FFmpeg وغيرها، وبعضها ينفذ عمليات **لا يمكن التراجع عنها** مثل حذف الملفات، إلغاء تعديلات Git، أو إنهاء العمليات قسراً.

- **راجع دائماً** المعاملات المدخلة (خصوصاً مسارات الملفات والمجلدات) قبل الضغط على تشغيل، لتفادي حذف أو تعديل بيانات غير مقصودة.
- بعض الأدوات (مثل `git.discard.clean` و `system.empty.folders.nuker` و `dotnet.nuget.cache.nuker`) تتطلب **تأكيداً صريحاً** ضمن معاملاتها؛ لا تفعّلها إلا بعد التأكد التام من المسار المستهدف.
- يُنصح بأخذ نسخة احتياطية من أي مجلد مهم قبل تجربة أداة تعديل أو حذف عليه لأول مرة.
- لا يتحمل ناشر المستودع (`Reda6Dev`) أو المساهمون فيه أي مسؤولية عن فقدان بيانات ناتج عن سوء استخدام الأدوات؛ الاستخدام على مسؤولية المستخدم الكاملة، وفقاً لشروط رخصة MIT ("AS IS" بدون أي ضمانات).
- قبل دمج أي أداة جديدة يُقدّمها مساهمون خارجيون، يُراجع فريق المستودع محتوى `argumentsTemplate` للتأكد من خلوه من أي أوامر ضارة أو مشبوهة.

---

<div align="center">

صُنع بـ ❤️ من أجل مجتمع مطوري الألعاب والمودرز — **OmniHub Official Tools Catalog**

</div>