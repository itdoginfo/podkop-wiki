---
layout: hextra-home
---

<section class="relative hx-flex hx-flex-col hx-items-center hx-justify-center hx-text-center hx-text-foreground hx-bg-background hx-overflow-hidden hx-rounded-lg hx-w-full">
  <div class="hx-absolute hx-inset-0 hx-pointer-events-none">
    <div class="hx-absolute -hx-top-32 -hx-right-48 hx-w-[640px] hx-h-[640px] hx-rounded-full hx-bg-primary/30 hx-blur-[180px]"></div>
    <div class="hx-absolute -hx-bottom-40 -hx-left-40 hx-w-[520px] hx-h-[520px] hx-rounded-full hx-bg-muted/40 hx-blur-[140px]"></div>
  </div>
  <div class="hx-relative hx-z-10 hx-max-w-2xl hx-px-6">
    <h1 class="hx-text-4xl sm:hx-text-4xl md:hx-text-5xl hx-font-extrabold hx-mb-4 hx-flex hx-items-center hx-justify-center hx-gap-2">
      Podkop
      {{< badge content="Beta" color="orange" >}}
    </h1>
    <p class="hx-text-lg hx-text-muted-foreground hx-mb-8">
      <strong>Маршрутизация трафика для OpenWrt</strong>
    </p>
    <p class="hx-text-lg hx-text-muted-foreground hx-mb-8">
      Направляйте нужные ресурсы в туннель, а остальное — напрямую. <br>
      Открытый инструмент на базе sing-box и FakeIP.
    </p>
    <a
  href="/docs/install/"
  class="main-cta-button hx-inline-flex hx-items-center hx-justify-center hx-gap-2 hx-text-base hx-font-semibold hx-rounded-xl hx-px-6 hx-py-3 hx-text-center hx-transition-colors hover:hx-brightness-95 focus:hx-outline-none focus:hx-ring-2 hx-ring-gray-300"
>
  Установить Podkop
  <svg
    class="hx-w-4 hx-h-4"
    aria-hidden="true"
    xmlns="http://www.w3.org/2000/svg"
    fill="none"
    viewBox="0 0 14 10"
  >
    <path
      stroke="currentColor"
      stroke-linecap="round"
      stroke-linejoin="round"
      stroke-width="2"
      d="M1 5h12m0 0L9 1m4 4L9 9"
    />
  </svg>
</a>
  </div>
</section>

<section class="hx-py-16 hx-w-full hx-mt-16">
  <div class="hx-w-full">
    <h2 class="hx-text-2xl sm:hx-text-3xl md:hx-text-4xl hx-font-bold hx-mb-10 hx-text-center">
      Популярные статьи
    </h2>
    <div class="hx-grid hx-grid-cols-1 md:hx-grid-cols-2 hx-gap-4">
      {{< card link="/docs/base-settings/" title="Основные настройки" icon="cog" >}}
      {{< card link="/docs/diagnostics/" title="Диагностика" icon="clipboard-list" >}}
      {{< card link="/docs/sections/" title="Несколько маршрутов" icon="cloud" >}}
      {{< card link="/docs/tunnels/awg_settings/" title="Amnezia WG" icon="shield-check" >}}
      {{< card link="/docs/client-dns/" title="DNS на клиентах" icon="globe-alt" >}}
      {{< card link="/docs/tunnels/" title="Туннели" icon="map" >}}
      {{< card link="/docs/clear-browser-cache/" title="Сброс кеша в браузере" icon="refresh" >}}
      {{< card link="/docs/fakeip/" title="FakeIP" icon="academic-cap" >}}
    </div>
  </div>
</section>
