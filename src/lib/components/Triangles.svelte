<script lang="ts">
	import { onMount } from 'svelte';

	interface Triangle {
		x: number;
		y: number;
		size: number;
		rotation: number;
		rotationModifier: number;
		color: string;
	}

	let canvas: HTMLCanvasElement;
	let context: CanvasRenderingContext2D;
	let triangles: Triangle[] = [];
	let numTriangles: number = 24;
	let colors: string[] = ['#2f27ce', '#d14747'];
	let animationFrameId: number;

	let width: number, height: number;
	let fallSpeed: number = 0.5;
	let rotationSpeed: number = 0.002;

	function resizeCanvas(): void {
		width = window.innerWidth;
		height = window.innerHeight;
		canvas.width = width;
		canvas.height = height;
	}

	function drawTriangle(x: number, y: number, size: number, color: string, rotation: number): void {
		context.fillStyle = color;
		context.beginPath();
		context.save();
		context.translate(x, y);
		context.rotate(rotation);
		context.moveTo(0, -size);
		context.lineTo(size, size);
		context.lineTo(-size, size);
		context.closePath();
		context.fill();
		context.restore();
	}

	function checkOverlap(x: number, y: number, size: number, triangles: Triangle[]): boolean {
		const minDistance = size * 2;
		return triangles.some((triangle) => {
			const dx = triangle.x - x;
			const dy = triangle.y - y;
			const distance = Math.sqrt(dx * dx + dy * dy);
			return distance < triangle.size + size;
		});
	}

	function updateTriangle(triangle: Triangle): void {
		triangle.y += fallSpeed;
		triangle.rotation += triangle.rotationModifier * rotationSpeed;

		// If triangle is off screen, recreate it at the top
		if (triangle.y > height + triangle.size) {
			let newX, newSize;
			let attempts = 0;
			const maxAttempts = 100;

			do {
				newX = Math.random() * width;
				newSize = Math.random() * 50 + 20;
				attempts++;
			} while (checkOverlap(newX, -newSize, newSize, triangles) && attempts < maxAttempts);

			if (attempts < maxAttempts) {
				triangle.x = newX;
				triangle.y = -newSize;
				triangle.size = newSize;
				triangle.rotation = Math.random() * Math.PI * 2;
				triangle.rotationModifier = Math.random() * 2 - 1; // Range: -1 to 1
				triangle.color = colors[Math.floor(Math.random() * colors.length)];
			}
		}
	}

	function draw(): void {
		context.clearRect(0, 0, width, height);
		triangles.forEach((triangle) => {
			updateTriangle(triangle);
			drawTriangle(triangle.x, triangle.y, triangle.size, triangle.color, triangle.rotation);
		});
		animationFrameId = requestAnimationFrame(draw);
	}

	onMount((): (() => void) => {
		canvas = document.getElementById('trianglesCanvas') as HTMLCanvasElement;
		context = canvas.getContext('2d') as CanvasRenderingContext2D;

		resizeCanvas();
		window.addEventListener('resize', resizeCanvas);

		for (let i = 0; i < numTriangles; i++) {
			let x, y, size;
			let attempts = 0;
			const maxAttempts = 100;

			do {
				x = Math.random() * width;
				y = Math.random() * height;
				size = Math.random() * 50 + 20;
				attempts++;
			} while (checkOverlap(x, y, size, triangles) && attempts < maxAttempts);

			if (attempts < maxAttempts) {
				triangles.push({
					x,
					y,
					size,
					rotation: Math.random() * Math.PI * 2,
					rotationModifier: Math.random() * 2 - 1,
					color: colors[Math.floor(Math.random() * colors.length)]
				});
			}
		}

		draw();

		return () => {
			window.removeEventListener('resize', resizeCanvas);
			cancelAnimationFrame(animationFrameId);
		};
	});
</script>

<canvas id="trianglesCanvas" class="fixed top-0 left-0 z-[-1] blur-[2px]"></canvas>
