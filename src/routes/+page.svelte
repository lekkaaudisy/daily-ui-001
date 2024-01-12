<script lang="ts">
    import { arrow, autoUpdate, computePosition, flip, offset, shift } from '@floating-ui/dom';
    import type { PopupSettings } from '@skeletonlabs/skeleton';
    import { popup, storePopup } from '@skeletonlabs/skeleton';
    import Icon from 'svelte-icons-pack/Icon.svelte';
    import AiFillEye from "svelte-icons-pack/ai/AiFillEye";
    import AiFillEyeInvisible from "svelte-icons-pack/ai/AiFillEyeInvisible";
    import FaSolidUnlockAlt from "svelte-icons-pack/fa/FaSolidUnlockAlt";
    import FaSolidUser from "svelte-icons-pack/fa/FaSolidUser";
    import IoMail from "svelte-icons-pack/io/IoMail";
    import { writable } from 'svelte/store';
    
    storePopup.set({ computePosition, autoUpdate, offset, shift, flip, arrow });
    
    const usernameRegex = /^[a-zA-Z0-9]{4,16}$/;
    const username = writable('');
    const emailRegex = /^\S+@\S+\.\S+$/;
    const email = writable('');
    const passwordRegex = /^.*(?=.{8,})(?=.*[a-zA-Z])(?=.*\d)(?=.*[!#$%&? "]).*$/;
    const password = writable('');
    const confirmPassword = writable('');
    
    const popupHover: PopupSettings = {
        event: 'hover',
        target: 'popupHover',
        placement: 'bottom'
    };
    
    $: passwordsMatch = $password === $confirmPassword;
    
    let user = {
      eyeOpen: false
    };
    
    function toggle() {
      user.eyeOpen = !user.eyeOpen; 
    }

    </script>
    
    <style>
        /* Default styles */
    .input-group input {
      width: 100%;
    }
    
    /* Adjust for smaller screens */
    @media (max-width: 375px) {
      .input-group input {
        max-width: 80%; 
      }
    }
    </style>
    
    <div class="container h-full mx-auto flex justify-center items-center min-h-[100vh] flex-col lg:flex-row rounded-3xl">
        <div class="rounded-3xl card w-full md:w-[80%] bg-initial overflow-hidden flex flex-col lg:flex-row lg:h-[650px] shadow-[4.0px_8.0px_8.0px_rgba(0,0,0,0.38)]">
            
            <!-- Image Section-->
            <header class="lg:w-[100%]"><img src="https://images.unsplash.com/photo-1508931133503-b1944a4ecdd5?ixid=M3w0Njc5ODF8MHwxfGFsbHx8fHx8fHx8fDE2ODc5NzY3OTV8&ixlib=rb-4.0.3" alt="Header Sign Up" class="bg-black/50 w-full lg:h-full object-cover"></header>

            <!-- Form Section -->
            <section class="p-4 md:p-0 md:px-8 lg:flex lg:flex-col lg:justify-center lg:items-center lg:w-[100%]">
                <h1 class="h1 mb-6 md:my-6">Sign Up</h1>
                <form method="POST" action="?/register" class="w-[100%]">
                    <label class="label space-y-3 mb-5" >
                        <!-- Input Username -->
                        <div class="input-group input-group-divider grid-cols-[auto_1fr_auto]">
                            <div class="input-group-shim"><Icon src={FaSolidUser} color=white/></div>
                            <input bind:value={$username} type="text" placeholder="Username" required class:valid={$username.length > 4 && usernameRegex.test($username)} class:invalid={$username.length > 4 && !usernameRegex.test($username)}/>
                        </div>
                        {#if $username.length > 0 && !usernameRegex.test($username)}<p>Username needs to be between 4 to 16 characters</p> {/if}

                        <!-- Input Email -->
                        <div class="input-group input-group-divider grid-cols-[auto_1fr_auto]">
                            <div class="input-group-shim"><Icon src={IoMail} color=white/></div>
                            <input bind:value={$email} type="email" placeholder="Email" required class:valid={$email.length > 0 && emailRegex.test($email)} class:invalid={$email.length > 0 && !emailRegex.test($email)}/>
                        </div>
                        {#if $email.length > 0 && !emailRegex.test($email)}<p>Invalid email</p> {/if}
    
                        <!-- Input Password -->
                        <div class="input-group input-group-divider grid-cols-[auto_1fr_auto]">
                            <div class="input-group-shim"><Icon src={FaSolidUnlockAlt} color=white/></div>
                            {#if user.eyeOpen}
                            <input bind:value={$password} type="text" placeholder="Password" id="password" required/>
                            {:else}
                            <input bind:value={$password} type="password" placeholder="Password" id="password" class="placeholder:text-sm" required/>
                            {/if}
                            <div title="Toggle visibility" >
                                <button type="button" class="btn-icon bg-initial" on:click={toggle}>
                                    {#if user.eyeOpen}
                                    <Icon src={AiFillEyeInvisible} color=white/>
                                    {:else}
                                    <Icon src={AiFillEye} color=white/>
                                    {/if}
                                </button>
                            </div>
                        </div>
                        {#if $password.length > 0 && !passwordRegex.test($password)}
                        <p class="text-xs md:text-base ">Passwords must have at least 8 characters and contain at least two of the following: uppercase letters, lowercase letters, numbers, and 2 different symbols.</p>
                        {/if}
    
                        <!-- Confirm Password -->
                        <div class="input-group input-group-divider grid-cols-[auto_1fr_auto]">
                            <div class="input-group-shim"><Icon src={FaSolidUnlockAlt} color=white/></div>
                            {#if user.eyeOpen}
                            <input bind:value={$confirmPassword} type="text" placeholder="Confirm Password" id="confirm_password" class="placeholder:text-sm" required class:valid={passwordsMatch} class:invalid={$confirmPassword.length > 0 && !passwordsMatch} />
                            {:else}
                            <input bind:value={$confirmPassword} type="password" placeholder="Confirm Password" id="confirm_password" class="placeholder:text-sm" required class:valid={passwordsMatch} class:invalid={$confirmPassword.length > 0 && !passwordsMatch} />
                            {/if}
                            <div title="Toggle visibility" >
                                <button type="button" class="btn-icon bg-initial" on:click={toggle}>
                                    {#if user.eyeOpen}
                                    <Icon src={AiFillEyeInvisible} color=white/>
                                    {:else}
                                    <Icon src={AiFillEye} color=white/>
                                    {/if}
                                </button>
                            </div>
                        </div>
                        {#if $confirmPassword.length > 0 && !passwordsMatch}
                        <p class="text-xs md:text-base ">Passwords Don't Match</p>
                        {/if}

                        <!-- Checkbox -->
                        <label class="flex items-center space-x-2">
                            <input class="checkbox" type="checkbox" required/>
                            <p class="text-xs ">I agree to the <span use:popup={popupHover} class="underline cursor-pointer">terms and conditions</span> as set out by the user agreement.</p>
                                <div class="card p-4 variant-filled-primary" data-popup="popupHover">
                                    <p>Terms and Conditions</p>
                                <div class="arrow variant-filled-primary" />
                            </div>
                        </label>
                    </label>

                    <!-- Submit Button -->
                    <div class="flex justify-center items-center">
                        <button type="submit" class="btn variant-soft-primary md:mb-6">
                            Sign Up
                        </button>
                    </div>
                </form>
            </section>
        </div>
    </div>