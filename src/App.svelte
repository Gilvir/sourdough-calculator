<script>
  import { Card, CardContent, CardHeader, CardTitle } from '$lib/components/ui/card';
  import { Label } from '$lib/components/ui/label';
  import { Input } from '$lib/components/ui/input';
  import { Slider } from '$lib/components/ui/slider';
  import { Separator } from '$lib/components/ui/separator';

  let flourWeight = 400;
  let hydration = [70];
  let starterPercent = [20];

  const saltPercent = 2;

  $: water = Math.round((flourWeight * hydration[0]) / 100);
  $: starter = Math.round((flourWeight * starterPercent[0]) / 100);
  $: salt = Math.round((flourWeight * saltPercent) / 100);
  $: totalDough = flourWeight + water + starter + salt;

  $: starterFlour = starter / 2;
  $: starterWater = starter / 2;
  $: totalFlour = flourWeight + starterFlour;
  $: totalWater = water + starterWater;
  $: trueHydration = ((totalWater / totalFlour) * 100).toFixed(1);

  $: sournessLevel = starterPercent[0] <= 12 ? "Very Sour"
    : starterPercent[0] <= 16 ? "Sour"
    : starterPercent[0] <= 22 ? "Mild"
    : "Less Sour";

  $: fermentationTime = starterPercent[0] < 15 ? "6-10 hours"
    : starterPercent[0] < 22 ? "4-6 hours"
    : "3-5 hours";
</script>

<div class="min-h-screen bg-neutral-50 p-4 md:p-8">
  <div class="max-w-4xl mx-auto space-y-6">

    <!-- Header -->
    <div class="text-center py-8">
      <h1 class="text-4xl font-light text-neutral-900 mb-2">Sourdough Calculator</h1>
      <p class="text-neutral-600">Baker's percentage calculator</p>
    </div>

    <!-- Ingredients -->
    <Card>
      <CardHeader>
        <CardTitle class="text-xl font-medium">Ingredients</CardTitle>
      </CardHeader>
      <CardContent class="space-y-6">

        <!-- Flour -->
        <div class="space-y-2">
          <Label for="flour" class="text-sm">Flour Weight (g)</Label>
          <Input
            id="flour"
            type="number"
            bind:value={flourWeight}
            min="1"
            class="text-xl h-12"
          />
          <p class="text-xs text-neutral-500">Base amount (100%)</p>
        </div>

        <Separator />

        <!-- Hydration -->
        <div class="space-y-3">
          <div class="flex justify-between items-start">
            <Label class="text-sm">Hydration</Label>
            <div class="text-right">
              <span class="text-xl font-medium">{hydration[0]}%</span>
              <p class="text-xs text-neutral-500">True: {trueHydration}%</p>
            </div>
          </div>
          <Slider
            bind:value={hydration}
            min={60}
            max={85}
            step={1}
          />
          <div class="flex justify-between text-xs text-neutral-400">
            <span>60%</span>
            <span>65%</span>
            <span>70%</span>
            <span>75%</span>
            <span>80%</span>
            <span>85%</span>
          </div>
        </div>

        <Separator />

        <!-- Starter -->
        <div class="space-y-3">
          <div class="flex justify-between items-start">
            <Label class="text-sm">Starter</Label>
            <div class="text-right">
              <span class="text-xl font-medium">{starterPercent[0]}%</span>
              <p class="text-xs text-neutral-500">{sournessLevel}</p>
            </div>
          </div>
          <Slider
            bind:value={starterPercent}
            min={10}
            max={30}
            step={1}
          />
          <div class="flex justify-between text-xs text-neutral-400">
            <span>10%</span>
            <span>20%</span>
            <span>30%</span>
          </div>
        </div>

      </CardContent>
    </Card>

    <!-- Recipe -->
    <Card>
      <CardHeader>
        <CardTitle class="text-xl font-medium">Recipe</CardTitle>
      </CardHeader>
      <CardContent>
        <div class="grid grid-cols-2 gap-4 mb-6">

          <div class="border rounded-lg p-4">
            <p class="text-xs text-neutral-500 mb-1">Flour</p>
            <p class="text-2xl font-medium">{flourWeight}g</p>
            <p class="text-xs text-neutral-400">100%</p>
          </div>

          <div class="border rounded-lg p-4">
            <p class="text-xs text-neutral-500 mb-1">Water</p>
            <p class="text-2xl font-medium">{water}g</p>
            <p class="text-xs text-neutral-400">{hydration[0]}%</p>
          </div>

          <div class="border rounded-lg p-4">
            <p class="text-xs text-neutral-500 mb-1">Starter</p>
            <p class="text-2xl font-medium">{starter}g</p>
            <p class="text-xs text-neutral-400">{starterPercent[0]}%</p>
          </div>

          <div class="border rounded-lg p-4">
            <p class="text-xs text-neutral-500 mb-1">Salt</p>
            <p class="text-2xl font-medium">{salt}g</p>
            <p class="text-xs text-neutral-400">{saltPercent}%</p>
          </div>

        </div>

        <div class="border-2 border-neutral-900 rounded-lg p-6 text-center">
          <p class="text-xs text-neutral-600 mb-1 uppercase tracking-wider">Total</p>
          <p class="text-4xl font-light">{totalDough}g</p>
        </div>
      </CardContent>
    </Card>

    <!-- Method -->
    <Card>
      <CardHeader>
        <CardTitle class="text-xl font-medium">Method</CardTitle>
      </CardHeader>
      <CardContent class="space-y-4">

        <div class="flex gap-4">
          <div class="flex-shrink-0 w-8 h-8 border rounded-full flex items-center justify-center text-sm font-medium">
            1
          </div>
          <div>
            <h3 class="font-medium mb-1">Mix</h3>
            <p class="text-sm text-neutral-600">
              Mix {flourWeight}g flour, {water}g water, {starter}g active sourdough starter (fed 4-8 hours ago, bubbly), and {salt}g salt until combined.
            </p>
          </div>
        </div>

        <div class="flex gap-4">
          <div class="flex-shrink-0 w-8 h-8 border rounded-full flex items-center justify-center text-sm font-medium">
            2
          </div>
          <div>
            <h3 class="font-medium mb-1">Bulk Fermentation</h3>
            <p class="text-sm text-neutral-600">
              Let dough rest at room temperature (21-24°C). Perform stretch and folds every 30 minutes for the first 2 hours (4 sets total): first time do <a href="https://www.youtube.com/watch?v=mwtTZK7_t08" target="_blank" rel="noopener" class="text-primary underline hover:opacity-80">vigorous</a> stretch and fold, then for remaining sets do <a href="https://www.youtube.com/watch?v=jrDy90gD710" target="_blank" rel="noopener" class="text-primary underline hover:opacity-80">gentle</a> stretch and folds. Then let rest undisturbed for {fermentationTime}. Dough should increase by 30-50%, feel airy and a little jiggly when gently shaking the bowl.
            </p>
          </div>
        </div>

        <div class="flex gap-4">
          <div class="flex-shrink-0 w-8 h-8 border rounded-full flex items-center justify-center text-sm font-medium">
            3
          </div>
          <div>
            <h3 class="font-medium mb-1">Shape</h3>
            <p class="text-sm text-neutral-600">
              Turn out onto lightly floured surface. <a href="https://www.youtube.com/watch?v=Op-LKk-i4zQ" target="_blank" rel="noopener" class="text-primary underline hover:opacity-80">Pre-shape</a> into a round, rest 20-30 minutes. Final shape into a <a href="https://www.youtube.com/watch?v=IWA0RAAsBHg" target="_blank" rel="noopener" class="text-primary underline hover:opacity-80">boule</a> or batard. Place seam-side up in a floured banneton.
            </p>
          </div>
        </div>

        <div class="flex gap-4">
          <div class="flex-shrink-0 w-8 h-8 border rounded-full flex items-center justify-center text-sm font-medium">
            4
          </div>
          <div>
            <h3 class="font-medium mb-1">Cold Proof</h3>
            <p class="text-sm text-neutral-600">
              Cover and refrigerate 8-48 hours. This develops flavor and makes scoring easier.
            </p>
          </div>
        </div>

        <div class="flex gap-4">
          <div class="flex-shrink-0 w-8 h-8 border rounded-full flex items-center justify-center text-sm font-medium">
            5
          </div>
          <div>
            <h3 class="font-medium mb-1">Bake</h3>
            <p class="text-sm text-neutral-600">
              Preheat Dutch oven at 230°C for 30 minutes. Turn out dough, score with a sharp blade. Bake covered 23 minutes at 230°C, then uncovered 10-20 minutes at 200°C until desired color.
            </p>
          </div>
        </div>

        <div class="flex gap-4">
          <div class="flex-shrink-0 w-8 h-8 border rounded-full flex items-center justify-center text-sm font-medium">
            6
          </div>
          <div>
            <h3 class="font-medium mb-1">Cool</h3>
            <p class="text-sm text-neutral-600">
              Transfer to a wire rack. Cool completely (at least 1 hour) before slicing.
            </p>
          </div>
        </div>

      </CardContent>
    </Card>

    <!-- Notes -->
    <Card class="bg-neutral-100 border-neutral-200">
      <CardHeader>
        <CardTitle class="text-xl font-medium">Notes</CardTitle>
      </CardHeader>
      <CardContent>
        <ul class="space-y-2 text-sm text-neutral-700">
          <li>• Assumes 100% hydration starter</li>
          <li>• Recipe hydration: {hydration[0]}% | True hydration: {trueHydration}%</li>
          <li>• Total flour: {Math.round(totalFlour)}g | Total water: {Math.round(totalWater)}g</li>
          <li>• Lower hydration (60-66%): easier to handle, tighter crumb</li>
          <li>• Higher hydration (76-85%): more open crumb</li>
          <li>• Less starter (10-14%): longer fermentation, more sour</li>
          <li>• More starter (23-30%): faster fermentation, milder</li>
        </ul>
      </CardContent>
    </Card>

  </div>
</div>
