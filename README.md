# رحلة الهضم (Digestion Journey) — الصف السادس الابتدائي

تطبيق تعليمي تفاعلي بجودة إنتاجية باللغة العربية (`RTL`) لطلاب الصف السادس الابتدائي، يشرح رحلة الطعام داخل الجهاز الهضمي للإنسان.

---

## 1. هيكل المشروع وفصل المسؤوليات (Clean Architecture)

```text
رحلة الهضم/
├── .env.example                           # قالب المتغيرات البيئية (بدون أي أسرار برمجية)
├── index.html                             # نقطة الدخول العربية (dir="rtl" lang="ar")
├── package.json                           # إعدادات الحزم وأوامر التشغيل والبناء
├── tsconfig.json                          # إعدادات TypeScript الصارمة (strict: true)
├── vite.config.ts                         # إعدادات Vite
├── docs/
│   ├── 01-repository-audit.md             # ١. تدقيق المستودع والبيئة والأصول المعلقة
│   ├── 02-implementation-plan.md          # ٢. خطة التنفيذ المرتبة حسب الاعتمادية
│   └── 03-requirements.md                 # ٣. وثيقة المتطلبات المرجعية المستمرة
└── src/
    ├── config/
    │   ├── env.ts                         # تحليل البيئة وفصل وضع العرض (Demo) عن الإنتاج (Production)
    │   └── unresolvedDependencies.ts      # سجل الأصول والمحتويات والإعدادات غير المحلولة
    ├── domain/
    │   ├── ids.ts                         # المعرّفات الثابتة (StudentId, ContentVersionId, LevelId, ActivityId, AttemptId)
    │   ├── types.ts                       # النماذج الأساسية للمجال
    │   └── rules.ts                       # القواعد النقية لحساب الإتقان وفتح المحطات تسلسلياً
    ├── content/
    │   ├── schema.ts                      # مخطط المحتوى التعليمي المستقل عن العرض
    │   └── digestionManifest.ts           # المحتوى العلمي المهيكل لمحطات الهضم الخمس (الإصدار v1.0.0)
    ├── persistence/
    │   ├── storagePort.ts                 # واجهة التخزين المستقلة
    │   └── localStorageAdapter.ts         # محول التخزين المحلي المعزول لكل وضع (Demo vs Production)
    ├── backend/
    │   ├── backendPort.ts                 # واجهة الاتصال بالخادم الخلفي
    │   └── backendFactory.ts              # مصنع محولات الخادم (LocalDemoBackendAdapter / ProductionBackendAdapter)
    ├── activities/
    │   └── ActivityRendererRegistry.tsx   # عارض الأنشطة التفاعلية وحساب نتائج المحاولات الفعلية
    ├── ui/
    │   └── layout/
    │       └── AppShell.tsx               # الواجهة الرئيسية العربية (RTL) ولوحة الجلسات والتبعيات
    └── styles/
        └── global.css                     # تصميم عربي متجاوب وواضح لطلاب الصف السادس
```

---

## 2. الفصل بين وضع العرض المحلي (Demo) ووضع الإنتاج (Production)

- **وضع العرض المحلي (`VITE_APP_MODE=demo`):**
  - يستخدم مساحة التخزين المعزولة `rehlat_alhadm_demo_v1`.
  - يعرض شريط تنبيه واضح بأنه وضع عرض محلي، ويوفر إمكانية إنشاء طالب تجريبي محلي للاختبار السريع.
- **وضع الإنتاج (`VITE_APP_MODE=production`):**
  - يستخدم مساحة التخزين المعزولة `rehlat_alhadm_prod_v1`.
  - يمنع أي أدوات بيانات تجريبية، ولا يضيف أي طلاب أو درجات أو لوحات صدارة وهمية.
  - يتحقق من اتصال الخادم الخلفي عبر `VITE_API_BASE_URL` ويبلغ بصراحة عن حالة الاتصال.

---

## 3. التشغيل والتحقق (Baseline Verification)

```bash
# تثبيت الحزم
npm install

# التحقق الصارم من الأنواع (TypeScript)
npm run typecheck

# بناء نسخة الإنتاج
npm run build

# تشغيل خادم التطوير المحلي
npm run dev
```
