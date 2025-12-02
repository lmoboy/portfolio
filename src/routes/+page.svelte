<script lang="ts">
   import Button from '$lib/components/UI/Button.svelte'
   import Image from '$lib/components/UI/Image.svelte'
   import ScrambleText from '$lib/components/UI/ScrambleText.svelte'
   import TextBox from '$lib/components/UI/TextBox.svelte'
   import Link from '$lib/components/UI/Link.svelte'
   import Input from '$lib/components/UI/Input.svelte'
   import Card from '$lib/components/UI/Card.svelte'
   import { onMount } from 'svelte'

   // Sample data
   const projects = [
      {
         title: 'Skill exchange platform',
         description:
            'Full-stack skill exchange platform with Svelte, Golang and PostgreSQL.',
         technologies: ['Svelte', 'Golang', 'PostgreSQL'],
         image: 'https://raw.githubusercontent.com/lmoboy/portfolio/refs/heads/master/src/lib/assets/images/skillswap.png',
         liveUrl: 'http://5o8e.l.time4vps.cloud/',
         githubUrl: 'https://github.com/lmoboy/SkillSwap',
      },
      {
         title: 'Github profanity checker',
         description:
            'Github profanity checker written in Python and the power of git api.',
         technologies: ['Python'],
         image: 'https://raw.githubusercontent.com/lmoboy/portfolio/refs/heads/master/src/lib/assets/images/profanity.png',
         liveUrl: 'disabled',
         githubUrl: 'https://github.com/lmoboy/profanity_checker',
      },
      {
         title: 'Wonky UI design',
         description:
            'Quite unconventional UI design with the power of svelte Svelte.',
         technologies: ['Svelte'],
         image: 'https://raw.githubusercontent.com/lmoboy/portfolio/refs/heads/master/src/lib/assets/images/wonkyui.png',
         liveUrl: 'https://wonky-ui-design.vercel.app/',
         githubUrl: 'https://github.com/lmoboy/wonky_ui_design',
      },
   ]

   const skills = [
      { name: 'JavaScript', level: 80 },
      { name: 'TypeScript', level: 60 },
      { name: 'React', level: 70 },
      { name: 'Vue.js', level: 50 },
      { name: 'Svelte', level: 90 },
      { name: 'SQL', level: 70 },
      { name: 'PHP', level: 80 },
      { name: 'Communication', level: 99 },
      { name: 'Git', level: 80 },
      { name: 'Docker', level: 40 },
      { name: 'Networking', level: 70 },
   ]

   let hoveredTech: string | null = null

   function scrollToSection(sectionId: string) {
      document.getElementById(sectionId)?.scrollIntoView({ behavior: 'smooth' })
   }
</script>

<svelte:head>
   <title>Daniel Developer - Full Stack Developer Portfolio</title>
   <meta
      name="description"
      content="Full-stack developer specializing in modern web technologies. Creating beautiful, functional, and user-friendly web applications."
   />
</svelte:head>

<!-- Hero Section -->
<section
   id="hero"
   class="min-h-screen flex items-center justify-center relative overflow-hidden bg-black"
>
   <div class="container mx-auto px-4 sm:px-6 text-center relative z-10">
      <div data-aos="fade-up" class="mb-8">
         <h1 class="text-4xl sm:text-6xl md:text-8xl font-bold text-white mb-6">
            <ScrambleText
               speed={0.5}
               text="Daniel Developer"
               className="text-white"
            />
         </h1>
         <p
            class="text-lg sm:text-xl md:text-2xl text-gray-300 mb-8 max-w-3xl mx-auto px-4"
         >
            Full-stack developer crafting beautiful, functional, and
            user-friendly web applications
         </p>
      </div>

      <div
         data-aos="fade-up"
         data-aos-delay="200"
         class="flex flex-col sm:flex-row gap-4 justify-center px-4"
      >
         <Button
            text="View My Work"
            className="text-base sm:text-lg px-6 sm:px-8 py-3 sm:py-4 w-full sm:w-auto"
            on:click={() => scrollToSection('projects')}
         />
         <Button
            text="Get In Touch"
            variant="secondary"
            className="text-base sm:text-lg px-6 sm:px-8 py-3 sm:py-4 w-full sm:w-auto"
            on:click={() => scrollToSection('contact')}
         />
      </div>
   </div>

   <!-- Scroll indicator -->
   <div
      data-aos="fade-up"
      data-aos-delay="400"
      class="absolute bottom-8 left-1/2 transform -translate-x-1/2"
   >
      <div class="animate-bounce">
         <svg
            class="w-6 h-6 text-white"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
         >
            <path
               stroke-linecap="round"
               stroke-linejoin="round"
               stroke-width="2"
               d="M19 14l-7 7m0 0l-7-7m7 7V3"
            ></path>
         </svg>
      </div>
   </div>
</section>

<!-- About Section -->
<section id="about" class="py-16 sm:py-20 bg-black pattern-bg">
   <div class="container mx-auto px-4 sm:px-6">
      <div class="max-w-4xl mx-auto">
         <div data-aos="fade-up" class="text-center mb-12 sm:mb-16">
            <h2
               class="text-3xl sm:text-4xl md:text-5xl font-bold text-white mb-6"
            >
               <ScrambleText text="About Me" className="text-white" />
            </h2>
            <p class="text-lg sm:text-xl text-gray-300 max-w-2xl mx-auto px-4">
               Passionate about creating digital experiences that make a
               difference
            </p>
         </div>

         <div class="grid md:grid-cols-2 gap-8 sm:gap-12 items-center">
            <div data-aos="fade-right" class="order-2 md:order-1">
               <Image
                  src="https://avatars.githubusercontent.com/u/54556836?v=4"
                  alt="Picture of me"
                  className="w-full h-auto shadow-2xl"
                  aspectRatio="5/6"
                  objectFit="cover"
               />
            </div>

            <div data-aos="fade-left" class="space-y-6 order-1 md:order-2">
               <TextBox className="p-6 sm:p-8">
                  <h3 class="text-xl sm:text-2xl font-bold text-white mb-4">
                     Hello, I'm Daniel
                  </h3>
                  <p class="text-gray-300 mb-4 text-sm sm:text-base">
                     I'm a passionate full-stack developer with 4* years of
                     experience creating digital pages for the tasks the
                     teachers gave us. I specialize in modern web technologies
                     and love turning complex ideas into simple, beautiful, and
                     intuitive user experiences.
                  </p>
                  <p class="text-gray-300 mb-6 text-sm sm:text-base">
                     When I'm not coding, you can find me playing video games,
                     watching movies, or spending time with my friends. On
                     special occasions you can find me playing my bass guitar or
                     trying to create complex C++ projects.
                  </p>
                  <span class="text-gray-400 text-sm p-2">
                     *: Only my vocational school experience from the very first
                     year.
                  </span>
                  <div class="flex flex-col sm:flex-row gap-4">
                     <Button
                        text="View my github"
                        variant="secondary"
                        className="w-full sm:w-auto"
                        on:click={() =>
                           window.open('https://github.com/lmoboy', '_blank')}
                     />
                     <!-- <Button text="Contact me" className="w-full sm:w-auto" /> -->
                  </div>
               </TextBox>
            </div>
         </div>
      </div>
   </div>
</section>

<!-- Skills Section -->
<section id="skills" class="py-20 bg-black">
   <div class="container mx-auto px-6">
      <div class="max-w-6xl mx-auto">
         <div data-aos="fade-up" class="text-center mb-16">
            <h2 class="text-4xl md:text-5xl font-bold text-white mb-6">
               <client:only>
                  <ScrambleText
                     text="Skills & Expertise"
                     className="text-white"
                  />
               </client:only>
            </h2>
            <p class="text-xl text-gray-300 max-w-2xl mx-auto">
               Technologies and tools I work with to bring ideas to life
            </p>
         </div>

         <div class="grid md:grid-cols-2 gap-12">
            <div data-aos="fade-right">
               <TextBox className="p-8">
                  <h3 class="text-2xl font-bold text-white mb-8">
                     Technical Skills
                  </h3>
                  <div class="space-6 grid grid-cols-2 gap-4">
                     {#each skills as skill}
                        <div
                           data-aos="fade-up"
                           data-aos-delay={skill.level}
                           role="button"
                           tabindex="0"
                           on:mouseenter={() => (hoveredTech = skill.name)}
                           on:mouseleave={() => (hoveredTech = null)}
                        >
                           <div class="flex justify-between items-center mb-2">
                              <span
                                 class="text-white font-medium transition-colors duration-300 {hoveredTech ==
                                 null
                                    ? 'opacity-100'
                                    : hoveredTech == skill.name
                                      ? ''
                                      : 'opacity-30'}">{skill.name}</span
                              >
                              <span
                                 class="text-gray-400 transition-colors duration-300 {hoveredTech ==
                                 null
                                    ? 'opacity-100'
                                    : hoveredTech == skill.name
                                      ? ''
                                      : 'opacity-30'}">{skill.level}%</span
                              >
                           </div>
                           <div
                              class="w-full bg-gray-700 border-2 border-white h-3 transition-all duration-300 {hoveredTech ==
                              null
                                 ? 'opacity-100'
                                 : hoveredTech == skill.name
                                   ? ''
                                   : 'opacity-30'}"
                           >
                              <div
                                 class="h-full transition-all duration-1000 ease-out bg-white {hoveredTech ==
                                 null
                                    ? 'opacity-100'
                                    : hoveredTech == skill.name
                                      ? ''
                                      : 'opacity-30'}"
                                 style="width: {skill.level}%"
                              ></div>
                           </div>
                        </div>
                     {/each}
                  </div>
               </TextBox>
            </div>

            <div data-aos="fade-left">
               <TextBox className="p-8">
                  <h3 class="text-2xl font-bold text-white mb-8">
                     Technologies
                  </h3>
                  <div class="flex flex-wrap gap-3">
                     {#each skills as tech, index}
                        <span
                           class="px-4 py-2 border-2 border-white bg-black text-white text-sm font-medium transition-all duration-300"
                           data-aos="zoom-in"
                           data-aos-delay={index * 50}
                           role="button"
                           tabindex="0"
                           on:mouseenter={() => (hoveredTech = tech.name)}
                           on:mouseleave={() => (hoveredTech = null)}
                        >
                           {tech.name}
                        </span>
                     {/each}
                  </div>
               </TextBox>
            </div>
         </div>
      </div>
   </div>
</section>

<!-- Projects Section -->
<section id="projects" class="py-20 bg-black pattern-bg">
   <div class="container mx-auto px-6">
      <div class="max-w-6xl mx-auto">
         <div data-aos="fade-up" class="text-center mb-16">
            <h2 class="text-4xl md:text-5xl font-bold text-white mb-6">
               Featured Projects
            </h2>
            <p class="text-xl text-gray-300 max-w-2xl mx-auto">
               A showcase of my recent work and side projects
            </p>
         </div>

         <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
            {#each projects as project, index}
               <div data-aos="fade-up" data-aos-delay={index * 100}>
                  <Card
                     className="p-0 overflow-hidden hover:scale-105 transition-transform duration-300"
                  >
                     <div class="relative">
                        <Image
                           src={project.image}
                           alt={project.title}
                           className="w-full h-48 object-cover"
                           aspectRatio="16/9"
                           objectFit="cover"
                        />
                        <div
                           class="absolute inset-0 bg-black bg-opacity-50 opacity-0 hover:opacity-100 transition-opacity duration-300 flex items-center justify-center"
                        >
                           <div class="flex gap-4">
                              <Button
                                 text="Live Demo"
                                 size="sm"
                                 disabled={project.liveUrl == 'disabled'}
                                 on:click={() =>
                                    project.liveUrl == 'disabled'
                                       ? null
                                       : window.open(project.liveUrl, '_blank')}
                              />
                              <Button
                                 text="GitHub"
                                 variant="secondary"
                                 size="sm"
                                 disabled={project.githubUrl == 'disabled'}
                                 on:click={() =>
                                    project.githubUrl == 'disabled'
                                       ? null
                                       : window.open(
                                            project.githubUrl,
                                            '_blank',
                                         )}
                              />
                           </div>
                        </div>
                     </div>
                     <div class="p-6">
                        <h3 class="text-xl font-bold text-white mb-3">
                           {project.title}
                        </h3>
                        <p class="text-gray-300 mb-4">{project.description}</p>
                        <div class="flex flex-wrap gap-2 mb-4">
                           {#each project.technologies as tech}
                              <span
                                 class="px-3 py-1 border border-white bg-black text-white text-xs hover:bg-white hover:text-black transition-all duration-300"
                              >
                                 {tech}
                              </span>
                           {/each}
                        </div>
                        <div class="flex gap-4">
                           <Link
                              href={project.liveUrl}
                              text="View Live"
                              variant="primary"
                              disabled={project.liveUrl == 'disabled'}
                           />
                           <Link
                              href={project.githubUrl}
                              text="GitHub"
                              variant="ghost"
                           />
                        </div>
                     </div>
                  </Card>
               </div>
            {/each}
         </div>
      </div>
   </div>
</section>

<!-- Contact Section -->
<section id="contact" class="py-20 bg-black">
   <div class="container mx-auto px-6">
      <div class="max-w-4xl mx-auto">
         <div data-aos="fade-up" class="text-center mb-16">
            <h2 class="text-4xl md:text-5xl font-bold text-white mb-6">
               Let's Work Together
            </h2>
            <p class="text-xl text-gray-300 max-w-2xl mx-auto">
               Ready to bring your ideas to life? Let's discuss your next
               project
            </p>
         </div>

         <div class="grid md:grid-cols-2 gap-12">
            <div data-aos="fade-right">
               <TextBox className="p-8">
                  <h3 class="text-2xl font-bold text-white mb-6">
                     Get In Touch
                  </h3>
                  <div class="space-y-4">
                     <div class="flex items-center gap-4">
                        <div
                           class="w-12 h-12 border-2 border-white bg-black flex items-center justify-center"
                        >
                           <svg
                              class="w-6 h-6 text-white"
                              fill="none"
                              stroke="currentColor"
                              viewBox="0 0 24 24"
                           >
                              <path
                                 stroke-linecap="round"
                                 stroke-linejoin="round"
                                 stroke-width="2"
                                 d="M3 8l7.89 4.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"
                              ></path>
                           </svg>
                        </div>
                        <div>
                           <p class="text-white font-medium">Email</p>
                           <p class="text-gray-300">steamgoyu19@gmail.com</p>
                        </div>
                     </div>

                     <div class="flex items-center gap-4">
                        <div
                           class="w-12 h-12 border-2 border-white bg-black flex items-center justify-center"
                        >
                           <svg
                              class="w-6 h-6 text-white"
                              fill="none"
                              stroke="currentColor"
                              viewBox="0 0 24 24"
                           >
                              <path
                                 stroke-linecap="round"
                                 stroke-linejoin="round"
                                 stroke-width="2"
                                 d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"
                              ></path>
                              <path
                                 stroke-linecap="round"
                                 stroke-linejoin="round"
                                 stroke-width="2"
                                 d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"
                              ></path>
                           </svg>
                        </div>
                        <div>
                           <p class="text-white font-medium">Location</p>
                           <p class="text-gray-300">Riga, Latvia</p>
                        </div>
                     </div>
                  </div>
               </TextBox>
            </div>

            <div data-aos="fade-left">
               <TextBox className="p-8">
                  <h3 class="text-2xl font-bold text-white mb-6">
                     Send a Message
                  </h3>
                  <span class="text-gray-400 text-sm p-2"
                     >Currently not working, email me directly</span
                  >
                  <form class="space-y-6">
                     <Input
                        disabled={true}
                        label="Name"
                        type="text"
                        placeholder="Your name"
                        className=""
                     />
                     <Input
                        disabled={true}
                        label="Email"
                        type="email"
                        placeholder="your@email.com"
                        className=""
                     />
                     <Input
                        disabled={true}
                        label="Message"
                        multiline={true}
                        rows={4}
                        placeholder="Tell me about your project..."
                        className=""
                     />
                     <Button
                        text="Send Message"
                        className="w-full"
                        disabled={true}
                     />
                  </form>
               </TextBox>
            </div>
         </div>
      </div>
   </div>
</section>

<!-- Footer -->
<footer class="bg-black py-12">
   <div class="container mx-auto px-6">
      <div class="max-w-4xl mx-auto text-center">
         <div data-aos="fade-up" class="mb-8">
            <TextBox className="p-8">
               <h3 class="text-2xl font-bold text-white mb-4">
                  Daniel Developer
               </h3>
               <p class="text-gray-300 mb-6">
                  Full-stack developer crafting digital experiences
               </p>
               <div class="flex justify-center gap-6">
                  <Link
                     href="https://github.com/lmoboy"
                     text="GitHub"
                     variant="ghost"
                  />
                  <Link
                     href="https://www.linkedin.com/in/daniils-vahramejevs-243415332/"
                     text="LinkedIn"
                     variant="ghost"
                  />
                  <!-- <Link href="https://twitter.com/steamgoyu19" text="Twitter" variant="ghost" /> -->
                  <Link
                     href="mailto:steamgoyu19@gmail.com"
                     text="Email"
                     variant="ghost"
                  />
               </div>
            </TextBox>
         </div>
         <div class="border-t-2 border-white pt-8">
            <p class="text-gray-400">
               &copy; 2025 Daniel Developer. All rights reserved.
            </p>
         </div>
      </div>
   </div>
</footer>
