<script lang="ts">
    import { page } from '@inertiajs/svelte';
    import { gsap } from 'gsap';
    import { ScrollTrigger } from 'gsap/ScrollTrigger';
    import { onMount } from 'svelte';
    import AppHead from '@/components/AppHead.svelte';
    import { Button } from '@/components/ui/button';

    let {
        _canRegister = true,
    }: {
        _canRegister?: boolean;
    } = $props();

    const _auth = $derived($page.props.auth);

    let cursor: HTMLElement;
    let cursorInner: HTMLElement;

    onMount(() => {
        if (typeof window !== 'undefined') {
            gsap.registerPlugin(ScrollTrigger);

            // Custom Cursor Logic
            const moveCursor = (e: MouseEvent) => {
                const { clientX: x, clientY: y } = e;
                gsap.to(cursor, {
                    x,
                    y,
                    duration: 0.5,
                    ease: 'power3.out'
                });
                gsap.to(cursorInner, {
                    x,
                    y,
                    duration: 0.1,
                    ease: 'power2.out'
                });
            };
            window.addEventListener('mousemove', moveCursor);
            gsap.set([cursor, cursorInner], { x: window.innerWidth / 2, y: window.innerHeight / 2 });

            // Hero Entrance
            const tl = gsap.timeline({ defaults: { ease: 'expo.out', duration: 1.2 } });
            tl.from('.nav-item', { y: -20, autoAlpha: 0, stagger: 0.08 })
              .from('.hero-reveal-text span', { 
                  y: 80, 
                  autoAlpha: 0, 
                  stagger: 0.012, 
                  rotationZ: 2,
                  transformOrigin: '0% 0% -50'
              }, '-=0.8')
              .from('.hero-content-elem', { y: 20, autoAlpha: 0, stagger: 0.08 }, '-=0.8');

            // Soft Parallax
            gsap.to('.parallax-bg', {
                scrollTrigger: {
                    trigger: 'body',
                    start: 'top top',
                    end: 'bottom bottom',
                    scrub: 1
                },
                y: 100,
                ease: 'none'
            });

            // Cinematic Reveals
            gsap.utils.toArray('.reveal-cinematic').forEach((elem: any) => {
                gsap.from(elem, {
                    scrollTrigger: {
                        trigger: elem,
                        start: 'top 94%',
                        toggleActions: 'play none none reverse'
                    },
                    y: 30,
                    autoAlpha: 0,
                    duration: 0.8,
                    ease: 'power3.out'
                });
            });

            return () => {
                window.removeEventListener('mousemove', moveCursor);
            };
        }
    });

    const handleInteraction = (e: MouseEvent, scale = 1.3) => {
        gsap.to(cursor, { 
            scale, 
            backgroundColor: 'rgba(255,255,255,0.1)', 
            borderColor: 'rgba(255,255,255,0.2)', 
            duration: 0.4,
            ease: 'power2.out'
        });
        gsap.to(cursorInner, { scale: 0.5, duration: 0.3 });
        
        const btn = e.currentTarget as HTMLElement;
        const rect = btn.getBoundingClientRect();
        const x = e.clientX - rect.left - rect.width / 2;
        const y = e.clientY - rect.top - rect.height / 2;
        
        gsap.to(btn, {
            x: x * 0.15,
            y: y * 0.15,
            scale: 1.02,
            duration: 0.4,
            ease: 'power2.out'
        });
    };

    const resetInteraction = (e: MouseEvent) => {
        gsap.to(cursor, { 
            scale: 1, 
            backgroundColor: 'transparent', 
            borderColor: 'white', 
            duration: 0.4,
            ease: 'elastic.out(1, 0.3)'
        });
        gsap.to(cursorInner, { scale: 1, duration: 0.3 });
        
        gsap.to(e.currentTarget, {
            x: 0,
            y: 0,
            scale: 1,
            duration: 0.6,
            ease: 'elastic.out(1, 0.3)'
        });
    };

    const services = [
        { id: '01', title: 'Software Solutions', desc: 'Enterprise architectures built for infinite scale and continental resilience.' },
        { id: '02', title: 'Data Analysis', desc: 'Proprietary pipelines distilling logic from multi-petabyte datasets.' },
        { id: '03', title: 'ML/AI', desc: 'Neural architectures optimized for high-throughput sovereign environments.' },
        { id: '04', title: 'Agentic AI', desc: 'Autonomous intelligence clusters executing complex logic with zero intervention.' },
        { id: '05', title: 'Embedded Logic', desc: 'Hardened firmware integration for critical industrial and defense infrastructures.' },
        { id: '06', title: 'IOT Ecosystems', desc: 'Sovereign data lanes connecting vast networks of sensors and controllers.' }
    ];

    const footerLinks = [
        {
            title: 'Core',
            links: [
                { name: 'Architecture', href: '#' },
                { name: 'Neural Hub', href: '#' },
                { name: 'Edge Devices', href: '#' },
                { name: 'Sovereign Data', href: '#' }
            ]
        },
        {
            title: 'Philosophy',
            links: [
                { name: 'Manifesto', href: '#' },
                { name: 'Sovereignty', href: '#' },
                { name: 'Direct Line', href: '#' },
                { name: 'Audit Logs', href: '#' }
            ]
        },
        {
            title: 'Legal',
            links: [
                { name: 'Privacy Protocols', href: '#' },
                { name: 'Terms of Autonomy', href: '#' },
                { name: 'Licensing', href: '#' }
            ]
        },
        {
            title: 'Connect',
            links: [
                { name: 'GitHub', href: '#' },
                { name: 'X / Twitter', href: '#' },
                { name: 'LinkedIn', href: '#' },
                { name: 'Direct Channel', href: '#' }
            ]
        }
    ];

    let uptime = $state(99.999);
    onMount(() => {
        const interval = setInterval(() => {
            uptime = 99.99 + Math.random() * 0.009;
        }, 3000);

        return () => clearInterval(interval);
    });
</script>

<AppHead title="Afrasynth | Sovereign Intelligence" />

<!-- Custom Cursor -->
<div bind:this={cursor} class="pointer-events-none fixed left-0 top-0 z-[9999] h-12 w-12 -translate-x-1/2 -translate-y-1/2 rounded-full border-2 border-white mix-blend-difference block transition-transform duration-500 ease-out"></div>
<div bind:this={cursorInner} class="pointer-events-none fixed left-0 top-0 z-[9999] h-2 w-2 -translate-x-1/2 -translate-y-1/2 rounded-full bg-white mix-blend-difference block"></div>

<div class="relative flex min-h-screen w-full flex-col bg-[#fbfbfb] text-[#050505] font-sans selection:bg-[#050505] selection:text-white overflow-x-hidden">
    <!-- Navigation -->
    <nav class="fixed top-0 z-50 flex w-full items-center justify-between px-8 py-4 md:px-16 border-b border-black/5 bg-white/80 backdrop-blur-xl">
        <div class="nav-item flex items-center gap-2">
            <span class="font-bebas text-5xl tracking-tighter italic">AFRASYNTH</span>
        </div>
        <div class="hidden gap-16 text-[10px] font-black uppercase tracking-[0.5em] md:flex">
            {#each ['Capabilities', 'Protocol', 'Manifesto'] as link (link)}
                <a href="#{link.toLowerCase()}" onmouseenter={(e) => handleInteraction(e, 1.1)} onmouseleave={resetInteraction} class="hover:opacity-100 transition-opacity py-2">{link}</a>
            {/each}
        </div>
        <div class="nav-item">
            <Button 
                onmousemove={(e) => handleInteraction(e, 1.1)} 
                onmouseleave={resetInteraction}
                variant="outline" 
                class="rounded-none border-[1.5px] border-black bg-transparent hover:bg-black hover:text-white text-[10px] font-black uppercase tracking-[0.4em] px-10 py-5 shadow-[6px_6px_0px_0px_rgba(0,0,0,1)] active:shadow-none transition-all duration-300"
            >
                Connect
            </Button>
        </div>
    </nav>

    <main class="flex flex-col">
        <!-- Hero Section -->
        <section class="relative flex min-h-[90vh] flex-col items-center justify-center p-8 text-center border-b border-black/5 bg-[#fbfbfb] overflow-hidden pt-20">
            <div class="parallax-bg absolute inset-0 z-0 opacity-[0.03] pointer-events-none" style="background-image: linear-gradient(#000 1px, transparent 1px), linear-gradient(90deg, #000 1px, transparent 1px); background-size: 100px 100px;"></div>
            
            <div class="hero-content-elem hero-tag mb-8 z-10 transition-transform duration-700 hover:scale-110">
                <div class="inline-block border border-black/10 bg-black px-10 py-3 shadow-lg">
                    <span class="text-[10px] font-black tracking-[0.7em] uppercase text-white select-none">Sovereign Intelligence Architecture</span>
                </div>
            </div>

            <div class="hero-reveal-text mb-12 max-w-7xl px-6 z-10 relative">
                <h1 class="font-bebas text-[clamp(3.5rem,13vw,10rem)] leading-[0.8] tracking-tighter italic mb-12 overflow-hidden">
                    {#each "ARCHITECTING AFRICA’S DIGITAL INDEPENDENCE.".split(" ") as word (word)}
                         <span class="inline-block mr-[0.2em]">
                            {#each word.split("") as char, i (i)}
                                <span class="inline-block">{char}</span>
                            {/each}
                         </span>
                    {/each}
                </h1>
            </div>

            <div class="hero-content-elem hero-cta z-10 pt-4">
                <Button 
                    onmousemove={(e) => handleInteraction(e, 1.15)} 
                    onmouseleave={resetInteraction}
                    class="h-24 rounded-none border-[3px] border-black bg-white hover:bg-black text-black hover:text-white px-24 text-4xl font-bebas tracking-[0.4em] uppercase transition-all duration-500 shadow-[20px_20px_0px_0px_rgba(0,0,0,1)] active:translate-x-3 active:translate-y-3 active:shadow-none"
                >
                    Learn More
                </Button>
            </div>
        </section>

        <!-- 01 / Capabilities -->
        <section id="capabilities" class="bg-white border-b border-black/5 py-32 px-8 md:px-16 overflow-visible">
            <div class="container mx-auto">
                <div class="reveal-cinematic mb-28 space-y-6">
                    <span class="text-[12px] font-black tracking-[0.5em] opacity-30 uppercase">01 / Capabilities</span>
                    <h2 class="font-bebas text-8xl md:text-[12rem] italic leading-none tracking-tighter transition-all hover:tracking-wide">SYSTEMS LOGIC</h2>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-10">
                    {#each services as service (service.id)}
                        <div 
                            onmouseenter={(e) => handleInteraction(e, 1)} 
                            onmouseleave={resetInteraction}
                            class="reveal-cinematic group relative flex flex-col justify-between min-h-[440px] p-12 bg-white hover:bg-[#050505] transition-all duration-1000 cursor-none border border-black/5 rounded-sm overflow-hidden"
                        >
                             <div class="font-bebas text-5xl text-black/[0.05] group-hover:text-white/[0.1] transition-all duration-700">{service.id}</div>
                             <div class="space-y-6 relative z-10 transition-transform duration-700 group-hover:translate-x-3">
                                 <h3 class="font-bebas text-5xl group-hover:text-white transition-colors duration-700 tracking-tight uppercase italic">{service.title}</h3>
                                 <p class="text-black/40 font-light text-lg leading-relaxed group-hover:text-white/50 transition-colors duration-1000 uppercase tracking-widest">{service.desc}</p>
                             </div>
                             <div class="absolute bottom-0 left-0 h-1.5 w-0 bg-white group-hover:w-full transition-all duration-1000"></div>
                        </div>
                    {/each}
                </div>
            </div>
        </section>

        <!-- 02 / Protocol -->
        <section id="protocol" class="bg-[#050505] text-white p-8 border-b border-black py-40 relative overflow-hidden">
             <div class="absolute inset-0 opacity-[0.03] pointer-events-none" style="background-image: radial-gradient(circle, #fff 1px, transparent 1px); background-size: 60px 60px;"></div>
             
             <div class="container mx-auto relative z-10">
                <div class="reveal-cinematic mb-40 flex flex-col items-center text-center space-y-10 group">
                    <span class="text-[12px] font-black tracking-[0.6em] opacity-30 uppercase">02 / Protocol</span>
                    <div class="relative cursor-none">
                        <h2 class="font-bebas text-[10rem] md:text-[18rem] italic leading-none tracking-tighter opacity-10 transition-opacity group-hover:opacity-20">LOGIC</h2>
                        <span class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 text-[15px] font-black tracking-[1em] uppercase w-full transition-all duration-1000 group-hover:tracking-[1.1em]">Development Protocol</span>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-4 gap-16">
                    {#each ['Audit', 'Design', 'Deploy', 'Sovereignty'] as step, i (step)}
                        <div 
                            onmouseenter={(e) => handleInteraction(e, 1.1)}
                            onmouseleave={resetInteraction}
                            class="reveal-cinematic flex flex-col items-center text-center space-y-14 group cursor-none"
                        >
                             <div class="h-24 w-24 border-[1.5px] border-white/20 flex items-center justify-center font-bebas text-5xl group-hover:bg-white group-hover:text-black group-hover:border-white transition-all duration-700 rounded-full">0{i+1}</div>
                             <h3 class="font-bebas text-5xl italic tracking-widest uppercase transition-all duration-700 group-hover:scale-105">{step}</h3>
                             <div class="w-px h-24 bg-gradient-to-b from-white/40 to-transparent group-hover:h-32 transition-all duration-1000"></div>
                        </div>
                    {/each}
                </div>
             </div>
        </section>

        <!-- 03 / Manifesto -->
        <section id="manifesto" class="bg-white py-32 px-8 md:px-16 border-b border-black/5">
            <div class="container mx-auto flex flex-col items-start space-y-28">
                 <div class="reveal-cinematic space-y-10">
                     <span class="text-[12px] font-black tracking-[0.5em] opacity-30 uppercase">03 / Manifesto</span>
                     <div class="max-w-6xl">
                         <p class="text-4xl md:text-[7rem] font-bold leading-[0.95] tracking-tighter text-[#050505] uppercase hover:text-black transition-colors duration-700">
                            We build the <span class="bg-black text-white px-5 py-1 inline-block transition-transform hover:-rotate-1">core logic</span> that powers continental independence. <span class="italic text-black/15 hover:text-black/30 transition-colors">Just pure architectural sovereignty.</span>
                         </p>
                     </div>
                 </div>
                 
                 <div class="grid grid-cols-1 md:grid-cols-2 w-full gap-32 border-t border-black/5 pt-20">
                     {#each [{t: 'DESTINY BY DESIGN', d: 'Secure data lanes and high-performance clusters for the next century.'}, {t: 'AUTONOMOUS CORE', d: 'Vetted logic for absolute autonomy and continental security standards.'}] as item (item.t)}
                         <div class="reveal-cinematic space-y-6 border-l-[1.5px] border-black/10 pl-10 hover:border-black transition-colors duration-700">
                             <h4 class="font-bebas text-4xl italic tracking-wide">{item.t}</h4>
                             <p class="text-xl font-light opacity-50 uppercase tracking-[0.2em] leading-loose">{item.d}</p>
                         </div>
                     {/each}
                 </div>
            </div>
        </section>

        <!-- 04 / Connect -->
        <section class="bg-[#050505] py-40 text-white text-center relative overflow-hidden overflow-y-visible">
             <div class="parallax-bg absolute inset-0 opacity-[0.03] pointer-events-none font-bebas text-[35rem] leading-none whitespace-nowrap flex items-center justify-center select-none uppercase">AFRICAN LOGIC</div>
             
             <div class="container mx-auto px-8 z-10 relative">
                 <div class="reveal-cinematic mb-16 flex flex-col items-center space-y-10">
                     <span class="text-[12px] font-black tracking-[0.7em] opacity-30 uppercase">04 / Connect</span>
                     <h2 class="font-bebas text-7xl md:text-[12rem] italic leading-none tracking-tighter hover:scale-105 transition-transform duration-1000">JOIN THE FRONT</h2>
                 </div>
                 
                 <div class="reveal-cinematic flex flex-col md:flex-row items-center justify-center gap-10 max-w-3xl mx-auto">
                      <input 
                        type="text" 
                        placeholder="IDENTITY@DOMAIN.ORG" 
                        class="w-full bg-transparent border-b-[2px] border-white/20 h-20 px-8 text-white font-black uppercase tracking-widest focus:outline-none focus:border-white placeholder:text-white/10 text-2xl transition-all duration-700" 
                      />
                      <Button 
                         onmousemove={(e) => handleInteraction(e, 1.15)} 
                         onmouseleave={resetInteraction}
                         class="h-20 w-full md:w-auto rounded-none border-[3px] border-white bg-transparent hover:bg-white text-white hover:text-black px-16 text-3xl font-bebas tracking-[0.4em] uppercase transition-all duration-700 shadow-[15px_15px_0px_0px_rgba(255,255,255,0.05)] active:shadow-none"
                      >
                         Secure System
                      </Button>
                 </div>
             </div>
        </section>
    </main>

    <!-- Professional Footer Overhaul - Vertical Restructuring -->
    <footer class="bg-white border-t border-black/5 py-32 md:py-48 px-8 md:px-16 lg:px-24">
        <div class="container mx-auto flex flex-col items-start gap-24 md:gap-32">
            
            <!-- Branding Header (Full Width) -->
            <div class="w-full flex flex-col md:flex-row items-start md:items-end justify-between gap-12 border-b border-black pb-12">
                <div class="reveal-cinematic">
                    <span class="font-bebas text-[8rem] md:text-[15rem] tracking-[0.05em] italic leading-none select-none transition-all duration-1000 hover:tracking-[0.1em] cursor-default inline-block">AFRASYNTH</span>
                </div>
                
                <!-- System Status Indicator integrated into header area -->
                <div class="reveal-cinematic flex flex-col gap-4 text-right mb-4">
                    <div class="flex items-center justify-end gap-3">
                        <div class="h-2 w-2 rounded-full bg-green-500 animate-pulse"></div>
                        <span class="font-black text-[10px] tracking-[0.4em] uppercase">SYSTEM_READY</span>
                    </div>
                    <div class="flex items-center justify-end gap-6 text-[10px] font-black tracking-[0.3em] uppercase opacity-30">
                        <span>{uptime.toFixed(3)}% UPTIME</span>
                        <span>LATENCY 14MS</span>
                    </div>
                </div>
            </div>

            <!-- Links Grid (Repositioned UNDER the branding) -->
            <div class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-5 gap-16 md:gap-24 w-full pt-12">
                <!-- Short Description / Mission -->
                <div class="reveal-cinematic lg:col-span-1 hidden lg:block pr-8 border-r border-black/5">
                    <p class="text-[11px] font-black uppercase tracking-[0.4em] leading-loose text-black/30 italic">
                        Architecting Africa's digital independence through verified system logic and sovereign neural clusters.
                    </p>
                </div>

                {#each footerLinks as column (column.title)}
                    <div class="reveal-cinematic flex flex-col gap-10">
                        <h4 class="text-black italic border-b border-black pb-4 w-fit text-[12px] font-black uppercase tracking-[0.4em]">{column.title}</h4>
                        <ul class="flex flex-col gap-6">
                            {#each column.links as link (link.name)}
                                <li>
                                    <a 
                                        href={link.href} 
                                        onmouseenter={(e) => handleInteraction(e, 1.05)} 
                                        onmouseleave={resetInteraction} 
                                        class="text-black/40 hover:text-black transition-all duration-500 text-[11px] font-black uppercase tracking-[0.5em] inline-block hover:translate-x-3"
                                    >
                                        {link.name}
                                    </a>
                                </li>
                            {/each}
                        </ul>
                    </div>
                {/each}
            </div>

            <!-- Sub-Footer Meta Info -->
            <div class="reveal-cinematic pt-24 mt-8 border-t border-black/5 flex flex-col lg:flex-row items-center justify-between gap-12 lg:gap-24 text-[10px] font-black uppercase tracking-[0.7em] opacity-20 w-full">
                <div class="flex flex-col md:flex-row items-center gap-8 md:gap-16 text-center md:text-left">
                    <span>© 2026 AFRASYNTH PBC</span>
                    <span class="hidden md:inline">•</span>
                    <span>ALL RIGHTS RESERVED</span>
                    <span class="hidden md:inline">•</span>
                    <span>v1.7.0_VERTICAL_CORE</span>
                </div>
                
                <div class="flex flex-wrap justify-center gap-12 md:gap-20">
                    <a href="#" class="hover:opacity-100 transition-opacity underline-offset-8">DATA INFRASTRUCTURE</a>
                    <a href="#" class="hover:opacity-100 transition-opacity underline-offset-8">SOVEREIGNTY PROTOCOL</a>
                </div>
            </div>
        </div>
    </footer>
</div>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap');

    :global(html), :global(body) {
        margin: 0;
        padding: 0;
        width: 100%;
        min-height: 100%;
        overflow-x: hidden;
        background-color: #fbfbfb;
        cursor: none !important;
    }

    /* Custom Scrollbar */
    :global(::-webkit-scrollbar) {
        width: 4px;
    }
    :global(::-webkit-scrollbar-track) {
        background: #fbfbfb;
    }
    :global(::-webkit-scrollbar-thumb) {
        background: rgba(0,0,0,0.2);
        border-radius: 10px;
    }

    .font-bebas {
        font-family: 'Bebas Neue', cursive, sans-serif !important;
    }
</style>
