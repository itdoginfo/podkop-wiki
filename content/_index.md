---
layout: hextra-home
---

<section class="relative flex flex-col items-center justify-center text-center text-foreground bg-background overflow-hidden rounded-lg hx-w-full">
   <div class="absolute inset-0 pointer-events-none">
      <div class="absolute -top-32 -right-48 w-[640px] h-[640px] rounded-full bg-primary/30 blur-[180px]"></div>
      <div class="absolute -bottom-40 -left-40 w-[520px] h-[520px] rounded-full bg-muted/40 blur-[140px]"></div>
   </div>
   <div class="relative z-10 max-w-2xl px-6">
      <h1 class="text-3xl sm:text-4xl md:text-5xl font-extrabold mb-4 flex items-center justify-center gap-2">
         Podkop
         {{< badge content="Beta" color="orange" >}}
      </h1>
      <p class="text-lg text-muted-foreground mb-8">
         <strong>Маршрутизация трафика для OpenWrt</strong>
      </p>
      <p class="text-lg text-muted-foreground mb-8">
         Направляйте нужные ресурсы в туннель, а остальное — напрямую. <br>
         Открытый инструмент на базе sing-box и FakeIP.
      </p>
      <a
         href="/docs/install/"
         class="main-cta-button inline-flex items-center justify-center text-base font-semibold rounded-xl px-6 py-3 text-center transition-colors hover:brightness-95 focus:outline-none focus:ring-2 ring-gray-300"
         >
         Установить Podkop
         <svg
            class="rtl:rotate-180 w-4 h-4 ms-3"
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

<section class="py-16 w-full">
  <div class="w-full">
    <h2 class="text-3xl font-bold mb-10 text-center">
      Популярные статьи
    </h2>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
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
