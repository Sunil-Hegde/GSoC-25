> Final report on the project by Sunil Hegde under RTEMS - Google Summer of Code 2025.

# Remove `set_vector()` accross all Architectures and BSPs

In my [proposal](./SunilHegde_Proposal_RTEMS_GSoc25.pdf), I have explained in detail why `set_vector()` had to be removed and  I had discussed on trying different solutions. While my contributions roughly remained in the same perimeter as discussed in my propoal, I did some additions.

## Challenges

While drafting my proposal, I had faced numerous challenges starting from building RTEMS tools due to various issues on my computer to understanding whys, wheres and hows of the project I had selected. 

Two issues, namely [#5215](https://gitlab.rtems.org/rtems/rtos/rtems/-/issues/5215) and [#4171](https://gitlab.rtems.org/rtems/rtos/rtems/-/issues/4171) discussed in detail on why the said function's removal was necessary. I have discussed the same in detail in these blog posts:
- [Why `set_vector()`?](https://blog.sunilhegde.tech/blog/why_set_vector)
- [Why remove `set_vector()`?](https://blog.sunilhegde.tech/blog/why_remove_set_vector)

While the necessaties of removing the functions were understood, It was decided to replace it with `rtems_interrupt_handler_install()` after suggestions from my mentors. The details of it are in this blog: [What should be the replacement for `set_vector()`?](https://blog.sunilhegde.tech/blog/what_should_be_the_replacement_for_set_vector)

All of my blogs majorly targeted the issues on SPARC specific implementation. The necessities of clearing and unmasking the interrupts are explained in this blog: [Why is it required to Clear and Unmask an Interrupt?](https://blog.sunilhegde.tech/blog/why_is_it_required_to_clear_and_unmask_an_interrupt)

I also found it difficult to trace the test failures to their origin during changes to SPARC bsps, and had to learn how interrupts are treated on SPARC boards and in RTEMS. After all the problems were targeted, my first merge request for SPARC boards was merged and soon NIOS II, m68k, microblaze, templates and cpukit followed.

## Merge requests

Here is the list of merged MRs:
- [sparc: removed uses, prototype and implementation of set_vector()](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/447)
- [bsps/nios2/nios2_iss: removed uses of set_vector()](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/560)
- [bsps/m68k: removed uses of set_vector() and its prototypes](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/624)
- [bsps/no_cpu: removed set_vector uses and prototype in the bsp template](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/659)

Pending MRs:
- [bsps/shared and powerpc: removed set_vector usage](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/638) - This MR introduces changes to drivers and hence has to be tested on physical boards.
- [cpukit/libgnat/ada_intrsupp.c: removed uses of set_vector()](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/658) - Adds changed related to Ada.

Changes to the documentation: [bsp-howto and cpu-supplement/sparc.md: Updates related to set_vector](https://gitlab.rtems.org/rtems/docs/rtems-docs/-/merge_requests/180)

All the updates are tracked on RTEMS EPICS [here](https://gitlab.rtems.org/groups/rtems/-/epics/26).

The final updates regarding all the changes are in [this](https://blog.sunilhegde.tech/blog/gsoc_update_set_vector_is_gone) blog.

## Learnings

Participating in GSoC 25 was more than fruitful to be honest. Before this summer I was confused about what actually interests me in the vast field of Computer Science. While my coursework was helpful in pointing me towards subjects like Operating Systems and Microcontrollers, this opportunity helped me place my foot into practical aspects of what I learnt in theory.

I learned programming in C during my first year at university, but I never really understood how full-fledged projects are built with it. Going through the RTEMS codebase, though overwhelming at first, helped me understand how large projects are structured and maintained. Along with the language I also learnt a lot about the build system, Git, and the CI pipeline.

Since my project was centered around interrupts, I had the chance to deep dive into the subject and truly understand how they are processed and why they are so critical in multiprocessor systems. Debugging interrupt-related issues was often challenging, and I found myself going down several rabbit holes while searching for solutions. These experiences taught me the value of approaching problems systematically and reinforced the importance of good documentation. At the same time, I also experimented with writing technical blogs to explain my work.

## Future work

Over the past three months, I have become familiar with different parts of the RTEMS codebase, and I plan to continue contributing beyond GSoC. One area I am particularly interested in is the [Memory Management Unit (MMU) support issue](https://gitlab.rtems.org/rtems/programs/gsoc/-/issues/5). I have already reviewed past work in this area and am working on setting up the environment to reproduce and build on it.

Just as I studied interrupt handling theory while drafting my proposal, I have now begun exploring the underlying concepts of MMUs and their importance in operating systems. Although my final exams are coming up soon, I plan to resume this work once they are completed and continue contributing to RTEMS.

## Conclusion 
None of this would have been possible without the invaluable guidance I received throughout the coding period from my mentors **Joel, Gedare, and the entire RTEMS community**. Beyond the project itself, I also learnt many important lessons, from maintaining backup branches to safeguard work, to the value of thorough documentation, and most importantly, the role of curiosity in software engineering. These lessons will stay with me and prove useful in the years ahead. I am deeply grateful to my mentors, the community, and to **Google** for providing me with this opportunity and experience.