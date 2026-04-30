<script>
    import { onMount } from 'svelte';
    
    let time = new Date();

    let seconds = $state(time.getSeconds());
    let maxHealth = 100;

    let winStr = $state("Win history:");
    let mainStr = $state("FIGHT!");

    let p1_attack = "AttackTrump.png"
    let p1_idle = "IdleTrump.png"
    let p1_die = "DamageTrump.png"
    let p1_dead = false
    let p1_state = $state(p1_idle)

    let p2_attack = "AttBarack.png"
    let p2_idle = "IdlleBarack.png"
    let p2_die = "DamageBarack.png"
    let p2_dead = false
    let p2_state = $state(p2_idle)

    let p1_health = $state(maxHealth)
    let p2_health = $state(maxHealth)


    function die(player){
        mainStr = "Game Over!"
        switch (player){
            case 1:
                p1_state = p1_die;
                p1_dead = true;
                winStr += "\nObama wins with " + p2_health + " health."
                break;
            case 2:
                p2_state = p2_die;
                p2_dead = true;
                winStr += "\nTrump wins with " + p1_health + " health."
                break;
        }
    }

    function damage(player){
        switch (player){
            case 1:
                p1_health -= 1;
                if (p1_health <= 0){
                    die(1)
                }
                break;
            case 2:
                p2_health -= 1;
                if (p2_health <= 0){
                    die(2)
                }
        }
    }

    function onKeyDown(e) {
		 switch(e.keyCode) {
			case 65:
                if (!p1_dead){
                    p1_state = p1_attack;
                    if(p2_health > 0){
                        damage(2);
                    }
                }
                break;
            case 76:
                if (!p2_dead){
                    p2_state = p2_attack;
                    if(p1_health > 0){
                        damage(1);
                    }
                }
                break;
            case 82:
                p1_state = p1_idle;
                p2_state = p2_idle;
                p1_health = maxHealth;
                p2_health = maxHealth;
                p1_dead = false;
                p2_dead = false;
                mainStr = "FIGHT!"
        }
    }

    function onKeyUp(e) {
		 switch(e.keyCode) {
			case 65:
                if (!p1_dead){
                    p1_state = p1_idle
                }
                break;
            case 76:
                if (!p2_dead){
                    p2_state = p2_idle
                }
                break;
        }
    }
</script>

<style>
:global(body) { margin: 0; padding: 0; }
.bg_whouse{
    max-height: 100vh;
    position: fixed;
    left:50%;
    top:50%;
    margin:0 auto;   
    transform: translate(-50%,-50%);
    z-index: -1;
}

.pres1{
    position: fixed;
    left: 50%;
    top: 75%;
    height: 50%;
    transform: translate(-80%,-50%);
}

.pres2{
    position: fixed;
    left: 50%;
    top:75%;
    height: 50%;
    transform: translate(-20%,-50%);
}

.p1_health{
    font-size: 400%;
    font-weight: 900;
}

.p2_health{
    position: fixed;
    left: 100%;
    top: 0%;
    transform: translate(-100%,0%);
    font-size: 400%;
    font-weight: 900;
}

.timer{
    position: fixed;
    left: 50%;
    top: 0%;
    transform: translate(-50%,0%);
}

.log{
    white-space: pre-line;
    font-size: 4vh;
    background-color: aliceblue;
    max-width: fit-content;
}

.mainStr{
    position: fixed;
    left: 50%;
    top: 10%;
    transform: translate(-50%,0%);
    font-size: 10vh;
}

</style>

<main>
    <div class="p1_health">{p1_health}</div>
    <div class="timer">{seconds}</div>
    <div class="p2_health">{p2_health}</div>
    <div class="log">{winStr}</div>
    <div class="mainStr">{mainStr}</div>
    <image src={p1_state} class="pres1"></image>
    <image src={p2_state} class="pres2"></image>
    <image src="GamePresident.png" class="bg_whouse"></image>
</main>

<svelte:window on:keydown|preventDefault={onKeyDown} on:keyup|preventDefault={onKeyUp}/>

