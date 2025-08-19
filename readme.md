<img src="./cover.png" width="450px" alt="অ্যাডভান্সড ওয়ার্ডপ্রেস প্লাগিন ডেভেলপমেন্ট" />



<!-- add template to read from github page -->
<div class="github-corner">
  <a href="https://hasanuzzamanbe.github.io/advanced-wordpress-plugin-development-bangla/" target="_blank" rel="noopener">
    <svg width="80" height="80" viewBox="0 0 250 250" style="fill:#151513; color:#fff; position: absolute; top: 0; border: 0; right: 0;" aria-label="View source on GitHub">
      <path d="M0,0 L115,115 L130,115 L142,142 L250,250 L250,0 Z"></path>
      <path d="M128.3,109.0 C113.8,99.7 119.0,89.6 119.0,89.6 C122.0,82.7 120.5,78.6 120.5,78.6 C119.2,72.0 123.4,76.3 123.4,76.3 C127.3,81.9 125.5,89.3 125.5,89.3 C122.9,100.9 130.6,105.4 134.4,106.2" fill="currentColor" style="transform-origin: 130px 106px;" class="octo-arm"></path>
      <path d="M115.0,115.0 C114.9,115.1 118.7,116.5 119.8,115.4 L133.7,101.6 C136.9,99.2 139.9,98.4 142.2,98.6 C133.8,88.0 127.5,74.4 143.8,58.0 C148.5,53.4 154.,51.,159.,51 C160.,51 161.,51 162.,51 C162.,51 162.,51 163.,51 C168.,51 173.,53.,177.,58 C196.,77.,183.,101.,183.,101 C182.,103.,181.,106.,179.,108 L165.,122 C164.,123.,165.,126.,165.,126 C165.,126 166.,127.,166.,128 C166.,129.,167.,130.,167.,130 L183.,146 C183.,146 L183.,146 C183.,146 L183.,146 Z" fill="currentColor" class="octo-body"></path>
    </svg
  </a>
</div>


<!-- ...existing code... -->

<!-- Stylish link to read the book -->
[📖 START READING](https://hasanuzzamanbe.github.io/advanced-wordpress-plugin-development-bangla/introduction.html/)










অ্যাডভান্সড ওয়ার্ডপ্রেস প্লাগিন ডেভেলপমেন্ট

### সূচিপত্র (Table of Contents)

বাংলায় প্রফেশনাল প্লাগিন তৈরির পূর্ণাঙ্গ গাইড

ভূমিকা

*   বইটি কাদের জন্য

*   এই বই থেকে কী কী শিখবেন

*   প্রয়োজনীয় টুলস ও পরিবেশ তৈরি


অধ্যায় ১: [মজবুত ভিত্তি স্থাপন (A Solid Foundation)](/chapter_1_Solid_Foundation.md)

*   ১.১ কেন সাধারণ ফাংশন-ভিত্তিক প্লাগিন যথেষ্ট নয়?

*   ১.২ আধুনিক PHP: Namespace এবং Autoloading (PSR-4)

*   ১.৩ Composer এর ব্যবহার

*   ১.৪ অবজেক্ট-ওরিয়েন্টেড প্রোগ্রামিং (OOP) এর সঠিক প্রয়োগ ও প্লাগিন আর্কিটেকচার


অধ্যায় ২: [হুকস (Hooks) এর গভীরে (A Deep Dive into Hooks)](./chapter_2_A_Deep_Dive_into_Hooks.md)

*   ২.১ প্রায়োরিটি (Priority) এবং আর্গুমেন্ট (Arguments) এর ব্যবহার

*   ২.২ হুক রিমুভ করা (remove\_action ও remove\_filter)

*   ২.৩ নিজের প্লাগিনকে এক্সটেনসিবল করা: কাস্টম হুকস তৈরি

*   ২.৪ do\_action() এবং apply\_filters() এর বাস্তব উদাহরণ


অধ্যায় ৩: [ডেটাবেস ম্যানেজমেন্ট (Database Management)](./chapter_3_Database_Management.md)

*   ৩.১ $wpdb এর নিরাপদ ব্যবহার এবং $wpdb->prepare()

*   ৩.২ কাস্টম ডেটাবেস টেবিল: কখন এবং কেন?

*   ৩.৩ dbDelta() ব্যবহার করে টেবিল তৈরি ও আপডেট করা

*   ৩.৪ CRUD অপারেশন (Create, Read, Update, Delete)


অধ্যায় ৪: [অ্যাডভান্সড API-এর ব্যবহার (Using Advanced APIs)](./chapter_4_Using_Advanced.md)

*   ৪.১ Settings API: পেশাদার অপশন পেজ তৈরি

*   ৪.২ REST API: কাস্টম এন্ডপয়েন্ট তৈরি এবং ব্যবহার

*   ৪.৩ Filesystem API: নিরাপদে ফাইল ম্যানেজ করা


অধ্যায় ৫: [গুটেনবার্গ ব্লক ডেভেলপমেন্ট (Gutenberg Block Development)](./chapter_5_Gutenberg_Block_Development.md)

*   ৫.১ ব্লক ডেভেলপমেন্টের পরিবেশ তৈরি (@wordpress/create-block)

*   ৫.২ একটি ব্লকের গঠন: block.json, edit.js এবং save.js

*   ৫.৩ অ্যাট্রিবিউটস এবং RichText কম্পোনেন্টের ব্যবহার

*   ৫.৪ ডাইনামিক ব্লক (সার্ভার-সাইড রেন্ডারিং)


অধ্যায় ৬: [নিরাপত্তা (Security)](./chapter_6_Security.md)

*   ৬.১ ওয়ার্ডপ্রেস নিরাপত্তার তিনটি গোল্ডেন রুল

*   ৬.২ Nonces: Cross-Site Request Forgery (CSRF) প্রতিরোধ

*   ৬.৩ ডেটা স্যানিটাইজেশন (ইনপুট ফিল্টারিং)

*   ৬.৪ আউটপুট এস্কেপিং: Cross-Site Scripting (XSS) প্রতিরোধ

*   ৬.৫ ব্যবহারকারীর অনুমতি যাচাই (Roles and Capabilities)


অধ্যায় ৭: [পারফরম্যান্স অপটিমাইজেশন (Performance Optimization)](./chapter_7_Performance_Optimization.md)

*   ৭.১ অ্যাসেট (CSS/JS) লোডিং অপটিমাইজ করা

*   ৭.২ Transients API ব্যবহার করে কোয়েরি ক্যাশ করা

*   ৭.৩ অ্যাসিঙ্ক্রোনাস অপারেশন: AJAX এবং WP-Cron

*   ৭.৪ ডেটাবেস কোয়েরি অপটিমাইজেশন


অধ্যায় ৮: [আন্তর্জাতিকীকরণ (Internationalization - i18n)](./chapter_8_Internationalization_i18n.md)

*   ৮.১ প্লাগিনকে অনুবাদযোগ্য করা (load\_plugin\_textdomain)

*   ৮.২ .pot ফাইল তৈরি এবং অনুবাদ প্রক্রিয়া

*   ৮.৩ জাভাস্ক্রিপ্টে অনুবাদ (wp\_localize\_script)


অধ্যায় ৯: [আধুনিক ডেভেলপমেন্ট ওয়ার্কফ্লো (Modern Development Workflow)](./chapter_9_Development_Workflow.md)

*   ৯.১ WP-CLI এর কার্যকর ব্যবহার

*   ৯.২ Git এবং GitHub ব্যবহার করে ভার্সন কন্ট্রোল

*   ৯.৩ WordPress.org রিপোজিটরিতে প্লাগিন ডিপ্লয়মেন্ট (SVN)


অধ্যায় ১০: [প্লাগিন থেকে আয় (Monetization)](./chapter_10_Monetization.md)

*   ১০.১ ফ্রিমিয়াম (Freemium) মডেলের কৌশল

*   ১০.২ লাইসেন্সিং এবং প্রো ভার্সন ম্যানেজমেন্ট

*   ১০.৩ পরিচিত প্ল্যাটফর্ম (Freemius, Appsero, Easy Digital Downloads)


পরিশিষ্ট (Appendix)

*   গুরুত্বপূর্ণ হুক এবং ফিল্টারের তালিকা

*   প্রয়োজনীয় কোড স্নিপেট

*   সহায়ক রিসোর্স এবং লিঙ্ক
