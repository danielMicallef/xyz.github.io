---
layout: post
title: "From High Fees to High Hopes: Building a Direct Booking Site with Astro"
lead: "How I built a direct booking website for my short-term rental, Seaview Apartment Xlendi, to save on hefty commission fees and learn a new framework in the process."
---

For the past three years, I've been renting out my apartment in the seaside village of Xlendi, Gozo. While platforms like Airbnb and Booking.com have been great for visibility, the 15-25% commission fees have always been a tough pill to swallow. As a software developer, I knew I could create a better, more cost-effective solution: a direct booking website.

## The Numbers Don't Lie

Let's break down the potential savings. My apartment is booked approximately 50% of the year, which translates to:

- **2023:** 185 Nights (50.7%)
- **2024:** 206 Nights (56.4%)
- **2025 (so far):** 165+ Nights (45%)

With an average stay of 4 nights at €92 per night, a typical booking generates €360 in revenue. A 15-25% commission on that is a significant chunk of change. By securing just one direct booking every couple of months, I could save €300-€400 a year. It is definitly not much, but it's a rewarding return for a side project.

## Getting the Word Out

A website is only as good as its traffic. In the first week after launching, Google Analytics showed a promising trickle of visitors. The primary sources have been:

- **Google Business Profile:** A must-have for any local business.
- **Direct Searches:** Guests finding the apartment on booking platforms and then searching for it directly to avoid fees.

In the future, I plan to explore more proactive marketing strategies, such as targeted ads on Google and Facebook, and building a social media presence to drive more organic traffic.

## A Learning Opportunity: Discovering Astro

This project wasn't just about saving money; it was also a chance to learn a new technology. I chose [Astro](https://astro.build/), a modern web framework that has been gaining a lot of traction. I was particularly drawn to its "island architecture," which allows you to build most of your site with static, content-focused components, and then "hydrate" small, interactive parts with your favorite UI framework (like React, Vue, or Svelte). This results in incredibly fast websites, which is a huge plus for user experience and SEO.

## What's Next?

The journey doesn't end here. I have two main goals for the future of this project:

1. **Improve UI/UX**: The goal till now was to have something shipped and available for upcoming guests. Going forward we'll need to do some extra work on the user interface to make [Seaview Apartment Xlendi](https://seaviewapartmentxlendi.com/) easier to navigate. 
2. **Themable and Dynamic:** I want to transform the website into a reusable theme where all content, including photos and property details, is fetched dynamically. This would allow me to quickly launch similar sites for other hosts who want to build a direct relationship with their guests.
3.  **Booking API Integration:** The ultimate goal is to add a fully-fledged booking engine. This would involve syncing calendars with other platforms to avoid double bookings and allowing guests to book and pay directly on the site. A seamless booking process is key to boosting conversion rates.
