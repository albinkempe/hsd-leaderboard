<script>
	export let title = '';
	export let progress = 90;
	export let total = 100;

	// new inputs (defaults match parent)
	export let step = 2500;
	export let maxTotal = 47500;

	// build rank list: Bronze 4 -> ... -> Diamond 1
	const league = ['Bronze', 'Silver', 'Gold', 'Platinum', 'Diamond'];
	const divisions = [4, 3, 2, 1];
	/**
	 * @type {any[]}
	 */
	const ranks = [];
	for (const m of league) for (const d of divisions) ranks.push(`${m} ${d}`);

	// index math (allow 0 as lowest bucket). clamp to max index.
	$: maxIndex = Math.floor(maxTotal / step);
	$: currentIndex = Math.min(Math.floor(progress / step), maxIndex);
	$: nextIndex = Math.min(currentIndex + 1, maxIndex);

	$: nextRankName = ranks[nextIndex] ?? ranks[ranks.length - 1];
	$: remaining = Math.max(nextIndex * step - progress, 0);

	$: progressPercent = Math.min((progress / Math.max(total, 1)) * 100, 100);
	$: isComplete = progressPercent === 100;

	// map metal -> color (adjust colors as desired)
	const colorMap = {
		Bronze: 'rgb(111,63,26)', // dark bronze
		Silver: 'rgb(108,108,108)', // dark silver / gray
		Gold: 'rgb(176,122,0)', // darker gold / amber
		Platinum: 'rgb(14,122,133)', // darker teal
		Diamond: 'rgb(10,79,138)' // darker blue
	};

	// derive metal from title (case-insensitive)
	$: metal = (title || '').split(' ')[0] || '';
	$: normalizedMetal = metal ? metal.charAt(0).toUpperCase() + metal.slice(1).toLowerCase() : '';
	// @ts-ignore
	$: rankColor = colorMap[normalizedMetal] ?? 'rgb(100, 100, 100)';

	// progress bar color follows the rank color; keep a distinct color when completed if you prefer
	$: progressColor = isComplete ? 'rgb(0, 123, 255)' : rankColor;

	/**
	 * @param {number} n
	 */
	function formatNumber(n) {
        if (n == null || Number.isNaN(n)) return '';
        const num = Math.round(n);
        return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
    }
</script>

<!-- ...existing code... -->
<div class="card">
	<p class="card-title">{title}</p>

	<div class="progress-bar">
		<div class="progress" style="width: {progressPercent}%; background-color: {progressColor};"></div>
		<p class="progress-text">{formatNumber(progress)} / {formatNumber(total)}</p>
	</div>

	<!-- show remaining points and next rank, or max notice when at top rank -->
	{#if currentIndex === maxIndex}
		<p class="progress-difference" style="color: {progressColor};">
			Max rank ({ranks[maxIndex]})
		</p>
	{:else}
		<p class="progress-difference" style="color: {rankColor};">
			{remaining} to {nextRankName}
		</p>
	{/if}
</div>

<style>
	.card {
		padding: 0;
		text-align: center;
		display: flex;
		flex-direction: column;
		align-items: center;
		border: 2px solid transparent;
		border-radius: 10px;
		transition:
			border 0.3s ease-in-out,
			box-shadow 0.3s ease-in-out;
	}

	.card-title {
		font-size: 1rem;
		margin: 0 0 0.7rem 0;
	}

.progress-bar {
        width: 200px;
        height: 30px;
        background-color: rgb(48, 48, 48);
        overflow: hidden;
        border-radius: 6px;
        position: relative;
    }

    .progress {
        position: absolute; /* keep fill behind the centered text */
        left: 0;
        top: 0;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        transition:
            width 0.6s cubic-bezier(0.2, 0.9, 0.3, 1),
            background-color 0.3s ease-in-out,
            box-shadow 0.3s ease-in-out;
        overflow: hidden; /* clip animations */
        box-shadow: inset 0 -2px 6px rgba(0, 0, 0, 0.35);
        z-index: 1;
    }

	/* subtler shimmer: lower opacity, slower, narrower band */
	.progress::before {
		content: '';
		position: absolute;
		top: 0;
		left: -80%;
		width: 160%;
		height: 100%;
		background: linear-gradient(
			90deg,
			rgba(255, 255, 255, 0) 0%,
			rgba(255, 255, 255, 0.06) 40%,
			rgba(255, 255, 255, 0.12) 50%,
			rgba(255, 255, 255, 0.06) 60%,
			rgba(255, 255, 255, 0) 100%
		);
		animation: shimmer 3s linear infinite;
		pointer-events: none;
		mix-blend-mode: overlay;
		filter: blur(0.6px);
	}

	/* very soft diagonal stripes for gentle texture */
	.progress::after {
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		width: 200%;
		height: 100%;
		background-image: repeating-linear-gradient(
			135deg,
			rgba(255, 255, 255, 0.03) 0px,
			rgba(255, 255, 255, 0) 10px
		);
		opacity: 0.12;
		transform: translateX(0);
		animation: stripes 6s linear infinite;
		pointer-events: none;
		mix-blend-mode: overlay;
	}

	@keyframes shimmer {
		to {
			left: 100%;
		}
	}

	@keyframes stripes {
		to {
			transform: translateX(-20px);
		}
	}

	@keyframes fadeInUp {
        from {
            opacity: 0;
            transform: translateY(6px);
        }
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }

.progress-text {
        color: white;
        font-size: 1rem;
        margin: 0;

        /* make the text span the whole bar and center with flexbox */
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 3;

        /* animate after the progress width finishes (0.6s) */
        animation: fadeInUp 320ms cubic-bezier(0.2,0.9,0.3,1) both;
        animation-delay: 600ms; /* match width transition duration */

        pointer-events: none;
    }

	.progress-difference {
		font-size: 0.9rem;
		margin-top: 0.7rem;
		margin-bottom: 0;
		color: rgb(83, 83, 83);
	}
</style>
