# Description

In video games a random map is a map generated randomly by the computer, usually in strategy games. Random maps are often the core of single and multiplayer gameplay, aside from story based campaigns that are often shipped with the game.

Each new game presents an unknown map, providing a new experience to the player and random maps typically follow an specific theme, for example, a naval theme with many small islands or a nuclear theme with an open desert with huge bomb holes. The type of random map can also influence the game's artificial intelligence, with the AI employing different strategies optimized for each random map.

# Introduction

This research project has been developed by Albert Ramisa for the subject Project II.

In this research the main objectives are the following:

- Do a research about Random Map Generation and the different implementations in videogames.
- Be able to understand how procedural maping is done and how we can create different types of maps taking into account all the different variations and characteristics.

![gif22](https://user-images.githubusercontent.com/72123380/166712406-050b0db6-89f8-4bbb-86ac-699cc3f8cf16.gif)
`Example of random generated islands`

# Market Study

Nowadays a video game demands several materials to compose a complex and large scene, this is a costly content in the development plan for every game development studio. So these algorithms are made to reduce the amount of work and in the end, time taken to create a game. From the player perspective they can make a huge impact on replayability, because the map you see your first game will not be the same the second time you play.

Maps generated randomly might not be the choice for everyone, it depends on the style and genre of your game to decide if its right or not for your project and there are some major downsides to consider too.

![gif2](https://user-images.githubusercontent.com/72123380/166711217-4ffd585e-4fca-4703-8bb2-383b54e9948c.gif)


`Example of random generated dungeons`

## Good aspects about Random Generated Maps

- It can save development time
- It increases replayability
- It can save memory usage
- It can enhance the exploration of your game

## Bad aspects about Random Generated Maps

- Worlds can feel repetitive
- It can also feel empty
- You may generate an unplayable world

## When you should use Random Generated Maps?

- If your project is an open world game
- If your project is a survival game
- If your project is based on exploration
- If your game focuses on trading

### Examples of Games using Procedural Maping

Now we will see how some of the most succesful and popular games used procedural maping

**Minecraft** : Like most 3D terrain in video games, it is entirely based on noise. When you begin the game, a random 64 bit number called a Seed is generated and this seed is used to generate the world. Different seeds will generate different worlds, and if you use the same seed you will always get the same result.

![image](https://user-images.githubusercontent.com/72123380/166725498-69b3f811-dd4a-4378-a116-f3aea4af55d7.png)

`Example of Minecraft as a game using procedural generation`

**Rust**: Similar procedural maping as minecraft, when the world is initially generated for the first time, the world creator will choose a random number, which determines the layout of the new world, and that number is called seed. Everything is randomized in the initial world generation, including radtown placement, roads, rivers, monuments, and biome placement.

![image](https://user-images.githubusercontent.com/72123380/166726596-1223b9d7-850e-4008-87eb-862c5c8f0dda.png)

`Example of Rust as a game using procedural generation`


**Spore**: A game quite popular in 2008, it was also famous for the usage of procedural maping in multiple levels, the initial planet was generated randomly but the space and galaxy surrounding your planet was also made with procedural generation.

![image](https://user-images.githubusercontent.com/72123380/166726322-fb724c7b-5893-41eb-bd6b-a6e40a1f7d80.png)

`Example of Spore as a game using procedural generation`

# Selected Approach

The tricky part in procedural generation is not to make things random, but to make them in a consistent way despite it’s randomness. There are two different types of ways to achieve that depending on your map:

![gifpr1](https://user-images.githubusercontent.com/72123380/166746366-772a16e3-19ac-4c26-a190-f9f8d02f78a0.gif)

`Outdoor Maping Example`

![gif22](https://user-images.githubusercontent.com/72123380/166747454-63c910df-03f4-4fd8-b2ca-2a959efa0385.gif)

`Indoor Maping Example`

To acomplish the procedure generation there will be a different method depending on wich of those two you select.

## Dungeon Generation

When making a dungeon you have to face the problem of filling the space with elements in a natural way. First step we have to do is to divide a plane into sets recursively. We divide until we can’t divide anymore or until we reach a maximum number of spaces.

![11111](https://user-images.githubusercontent.com/72123380/166748959-9097de97-3303-4ae0-b8ce-7feff94d58ca.PNG)

`Dividing a plane until we reach a maximum number of spaces`

We got a random division, but we don’t want the rooms to use the whole space, so let’s add a method to cut their borders recursively. We can achieve that giving a regular or non regular margin between space wall and room wall. And the last step is to add corridors, we are going to do this by recursively connecting each node with it’s sibling.

![image](https://user-images.githubusercontent.com/72123380/166749486-6aa9a0d2-5c67-448a-85d4-9341f196d87f.png)

`Creating the corridors to conect the different rooms`

## Perlin Noise

A common way to generate 2D maps is to use a noise function, such as perlin noise. The noise function look like this:

![image](https://user-images.githubusercontent.com/72123380/166792382-f1e302cd-7771-4fca-ae8c-e8be917f5f9a.png)

`Perlin noise multiple examples`

To create a random terrain we need to store a seed, a math formula and set a frequency.We assign each location on the map a number from 0.0 to 1.0. In this image, 0.0 is the water and 1.0 is the white snow. Once you have this values, you interpretate numbers to spawn terrains.

![image](https://user-images.githubusercontent.com/72123380/166802011-abed0768-0844-4112-9cdc-2ee43507b857.png)

`Example of Perlin noise applied to create a map terrain`

In order to implement the Perlin noise in the code we first have to create a FastNoise object and then set that object type to Perlin Noise:

```C++
 FastNoiseLite noise;
 
 noise.SetNoiseType(FastNoiseLite::NoiseType_Perlin);
```

Once we have that we can now create the seed and we can also set the frequency:

```C++
 noise.SetSeed(seed);
 
 noise.SetFrequency(0.02);
```

Now we need to store the values generated by Perlin Noise, we have to get noise at the coords x,y and store that value in app->map->height_map. 

Now we need to set the noise to be between 1 and 0, so the next step we have to do is to apply the formula given by the author of the library, (Noise + 1) *0.5 :

```C++
for (int x = 0; x < 100; x++)
	{
		for (int y = 0; y < 100; y++)
		{

			app->map->height_map[x][y] = (noise.GetNoise((float)x, (float)y) +1) * 0.5;

		}
	}
```
The final step is to draw the map, and remember, the values have to be between 0 and 1, being more dark the closest to 0 and more light the closest to 1, so the forest should go before the grass, the grass before the sand and the sand before the water:

```C++
    if (value > 0 && value < 0.2) app->render->DrawTexture(app->scene->forestTex,pos.x, pos.y, NULL, scale);
				else if (value > 0.2 && value < 0.4) app->render->DrawTexture(app->scene->grassTex, pos.x, pos.y, NULL, scale);
				else if (value > 0.4 && value < 0.6)  app->render->DrawTexture(app->scene->sandTex, pos.x, pos.y, NULL, scale);
				else if (value > 0.6 && value < 1)  app->render->DrawTexture(app->scene->waterTex, pos.x, pos.y, NULL, scale);
```

### Posible Improvements

A way to improve the random map generation is to add premade structures, changing the perlin function in order to have for example little villages or chests. Special landmarks can act as a rewarding system to the player and they help in improving the world playability and overall engagement and satisfaction of the player. 

Those structures must have a function to mesure the distance between one and the other or a function to mesure the probabilty of spawning,in other words, how random they are. Those functions are made to avoid overlapping those structures or to simply make them very rare to find, for example, we dont want a world plagued with rewarding chests every ten meters.

Another way to improve the perlin noise function is to add multiple levels on it, in that way you can be able to create very distinct and rich biomes and improve the overall replayability of your game.

### Exercise

Link to the template with the exercise created to test the knowledge about Random Map Generation:

https://github.com/Taks7/RandomMapGeneration/releases/tag/Exercises

Inside we can found a serie of TODO's in order to complete the template.

Here we have the solution of the exercises:

https://github.com/Taks7/RandomMapGeneration/releases/tag/Solution

### Citations
Information about the code implementation of a random map generator:
- https://github.com/Azgaar/Fantasy-Map-Generator

Detailed information on how to create a random dungeon:
- https://gamedevelopment.tutsplus.com/tutorials/create-a-procedurally-generated-dungeon-cave-system--gamedev-10099

Couple of useful videos to understand better how procedural generation works in video games:
- https://www.youtube.com/watch?v=ZZY9YE7rZJw&ab_channel=javidx9
- https://www.youtube.com/watch?v=jv6YT9pPIHw&ab_channel=BarneyCodes

Support library that has served to implement the code:

- [FastNoise Library](https://github.com/Auburn/FastNoiseLite)

Template used for the module:

- [Code template used as a base for the Random Map Generation](https://github.com/raysan5/game_project_template)
