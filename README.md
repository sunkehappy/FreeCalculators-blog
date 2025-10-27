# Free Calculators Tech Blog

> Documenting technical challenges, solutions, and best practices in building a multilingual calculator platform

## 📖 Project Overview

[Free Calculators](https://www.freecalculators.app) is a multilingual online calculator platform serving global users with professional calculation tools across mathematics, finance, legal, automotive engineering, and other domains. Built with modern technology stack and supporting 12 languages, we're committed to delivering fast, accurate, and user-friendly calculation experiences.

## 🛠 Technology Stack

### Core Technologies
- **Next.js 15** - React full-stack framework with App Router
- **React 19** - Latest React version with concurrent features
- **TypeScript** - Strict type checking for enhanced code quality
- **Tailwind CSS 4** - Atomic CSS framework for rapid UI development
- **DaisyUI** - Tailwind CSS component library

### Internationalization
- **next-intl** - Next.js internationalization solution
- **12 Language Support** - English, Chinese, Japanese, Korean, French, German, Spanish, Portuguese, Russian, Arabic, Hindi, Italian

### Performance Optimization
- **Static Export** - Using `output: 'export'` for static site generation
- **Image Optimization** - AVIF, WebP format support with multi-size adaptation
- **Caching Strategy** - Layered caching with strong cache for static assets, negotiation cache for pages
- **Code Splitting** - Automatic splitting by routes and components

### SEO & Structured Data
- **Schema.org Structured Data** - WebApplication, FAQ, HowTo, BreadcrumbList
- **Multilingual hreflang** - Proper language version markup
- **Open Graph & Twitter Cards** - Social media sharing optimization
- **Sitemap Generation** - Automatic multilingual sitemap generation

### Development Tools
- **ESLint + Prettier** - Code quality and formatting
- **pnpm** - Efficient package manager
- **Sentry** - Error monitoring and performance tracking
- **Supabase** - Backend services (optional)

## 🚀 Technical Highlights

### 1. Multilingual Architecture Design

We built a comprehensive multilingual solution:

```typescript
// Routing configuration
export const routing: RoutingConfig = {
  locales: ['en', 'zh', 'ja', 'ko', 'fr', 'de', 'es', 'pt', 'ru', 'ar', 'hi', 'it'],
  defaultLocale: 'en',
  pathnames: {
    '/': '/',
    '/calculator': {
      en: '/calculator',
      zh: '/计算器',
      ja: '/計算機',
      // ... other languages
    }
  }
};
```

**Challenges & Solutions:**
- **Challenge**: How to elegantly handle URL structures for different languages
- **Solution**: Using next-intl's pathnames configuration to support language-specific URL paths

### 2. Static Export Optimization

```typescript
// next.config.ts
const nextConfig: NextConfig = {
  output: isDev ? undefined : 'export', // Static export for production
  images: {
    unoptimized: true, // Static export requires disabling image optimization
    formats: ['image/avif', 'image/webp'],
  },
  experimental: {
    clientRouterFilter: false, // Reduce RSC payload
    staleTimes: {
      static: 300, // Static page cache time
    },
  },
};
```

**Technical Challenges:**
- **RSC Payload Issues**: React Server Components serialization problems during static export
- **Image Optimization Limitations**: Static export doesn't support Next.js built-in image optimization
- **Middleware Compatibility**: Development needs middleware, production needs static export

### 3. Complex Calculation Logic Implementation

The project includes various professional calculators, such as legal sentencing calculations and automotive engineering calculations:

```typescript
// Ohio sentencing calculation example
export function calculateOhioCreditTime(
  input: OhioCreditTimeInput,
  rules: OhioRules = DEFAULT_OHIO_RULES,
): OhioCreditTimeResult {
  // 1. Validate input
  const validationError = validateInput(input, rules);
  if (validationError) {
    throw new Error(validationError);
  }

  // 2. Calculate total sentence days
  const imposedDays = toDays({
    years: input.sentenceYears,
    months: input.sentenceMonths,
  });

  // 3. Apply jail credit
  const remainingDaysAfterJailCredit = applyJailCredit(
    imposedDays,
    input.jailCreditDays,
  );

  // 4. Calculate earned credit
  let earnedCreditDaysRaw = calcEarnedCredit(
    input.programMonthsParticipated,
    input.offenseCategory,
    rules,
  );

  // 5. Apply disciplinary loss
  const earnedCreditAfterDiscipline = applyDiscipline(
    earnedCreditDaysRaw,
    input.disciplinaryCreditLossDays,
  );

  // 6. Apply 15% cap
  const { cappedDays: finalEarnedCreditDays } = applyEarnedCap(
    earnedCreditAfterDiscipline,
    imposedDays,
    rules,
  );

  // 7. Calculate final days to serve
  const netDaysToServe = Math.max(
    0,
    remainingDaysAfterJailCredit - finalEarnedCreditDays,
  );

  return {
    imposedDays,
    netDaysToServe,
    earnedCreditDays: finalEarnedCreditDays,
    projectedCompletionDate: projectRelease(input.admissionDate, netDaysToServe),
  };
}
```

**Technical Challenges:**
- **Legal Complexity**: Vast differences in legal statutes across different states require precise modeling
- **Calculation Precision**: Date calculations and percentage calculations require high-precision handling
- **Edge Cases**: Various special circumstances require comprehensive error handling

### 4. SEO & Structured Data

```typescript
// Generate multiple types of structured data
export function generateMultipleStructuredData({
  locale,
  title,
  description,
  slug,
  calculatorType,
  features,
  faqs,
  howToSteps,
  breadcrumbItems,
}) {
  const structuredDataArray = [];

  // WebApplication structured data
  if (calculatorType) {
    structuredDataArray.push({
      '@context': 'https://schema.org',
      '@type': 'WebApplication',
      name: title,
      description,
      applicationCategory: 'UtilitiesApplication',
      applicationSubCategory: calculatorType,
      offers: {
        '@type': 'Offer',
        price: '0',
        priceCurrency: 'USD',
      },
      featureList: features || [],
    });
  }

  // FAQ structured data
  if (faqs && faqs.length > 0) {
    structuredDataArray.push({
      '@context': 'https://schema.org',
      '@type': 'FAQPage',
      mainEntity: faqs.map(faq => ({
        '@type': 'Question',
        name: faq.question,
        acceptedAnswer: {
          '@type': 'Answer',
          text: faq.answer,
        },
      })),
    });
  }

  return structuredDataArray;
}
```

**SEO Optimization Strategy:**
- **Multilingual hreflang**: Proper markup for different language versions
- **Structured Data**: Rich search result display
- **Performance Optimization**: Core Web Vitals optimization
- **Content Quality**: EEAT (Experience, Expertise, Authoritativeness, Trustworthiness) principles

### 5. Translation Management System

We developed a comprehensive translation management toolset:

```bash
# Check translation status
node scripts/check-translations.js

# Find missing translation keys
node scripts/check-translations.js missing

# Format translation files
node scripts/format-translations.js

# Clean duplicate keys and formatting issues
node scripts/clean-translations.js
```

**Management Challenges:**
- **Key-Value Consistency**: Ensuring consistent key-value structure across all language versions
- **Translation Quality**: Maintaining translation quality across 12 languages
- **Version Control**: Translation file version management and conflict resolution

## 📚 Blog Content

This repository documents technical articles from the development process, covering:

### Architecture Design
- [Technical Challenges in Building Multilingual Calculator Platform](./blog/site-architecture-blog/)
- Static Export vs SSR Trade-offs
- Internationalization Routing Design Best Practices

### Performance Optimization
- Next.js 15 Performance Optimization Tips
- Image Optimization with WebP/AVIF Implementation
- Caching Strategy Design and Implementation

### SEO Practices
- Structured Data Implementation Guide
- Multilingual SEO Optimization
- Core Web Vitals Optimization Experience

### Development Tools
- TypeScript Strict Mode Practices
- ESLint + Prettier Configuration Optimization
- Translation Management Automation Tools

## 🔧 Development Environment

### Requirements
- Node.js 18+
- pnpm 8+
- TypeScript 5+

### Quick Start

```bash
# Clone repository
git clone <repository-url>
cd free-calculators-tech-blog

# Install dependencies
pnpm install

# Start development server
pnpm dev

# Build production version
pnpm build

# Format code
pnpm format:all
```

### Translation Management

```bash
# Check translation completeness
node scripts/check-translations.js

# Add missing translations
node scripts/add-missing-translations.js

# Format all translation files
node scripts/format-translations.js
```

## 📈 Performance Metrics

- **Lighthouse Score**: 95+ (Performance, Accessibility, Best Practices, SEO)
- **Core Web Vitals**: All green
- **Page Load Time**: < 2s (mobile)
- **Image Optimization**: 40% loading speed improvement

## 🌍 Multilingual Support

| Language | Code | Status | Coverage |
|----------|------|--------|----------|
| English | en | ✅ Complete | 100% |
| Chinese | zh | ✅ Complete | 100% |
| Japanese | ja | ✅ Complete | 100% |
| Korean | ko | ✅ Complete | 100% |
| French | fr | ✅ Complete | 100% |
| German | de | ✅ Complete | 100% |
| Spanish | es | ✅ Complete | 100% |
| Portuguese | pt | ✅ Complete | 100% |
| Russian | ru | ✅ Complete | 100% |
| Arabic | ar | ✅ Complete | 100% |
| Hindi | hi | ✅ Complete | 100% |
| Italian | it | ✅ Complete | 100% |

## 🤝 Contributing Guidelines

We welcome contributions to technical articles! Please follow these guidelines:

1. **Article Structure**: Problem Description → Technical Solution → Implementation Details → Experience Summary
2. **Code Examples**: Provide complete, runnable code examples
3. **Multilingual**: Important articles should have English and Chinese versions
4. **Technical Depth**: In-depth analysis of technical challenges and solutions

## 📞 Contact

- **Project Homepage**: [https://www.freecalculators.app](https://www.freecalculators.app)
- **Technical Issues**: Discuss via GitHub Issues
- **Collaboration**: Welcome technical exchanges and cooperation

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

*Documenting technical growth, sharing development experience, building better calculator platforms* 🚀
