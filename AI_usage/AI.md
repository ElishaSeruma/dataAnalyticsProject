How i used Ai
I asked ai to help me structure out the repository.

I then asked Ai to look through all of the labs we did in class and create a Single source of truth a sort of map to guide my entire process from the practical work we did in class.

I then asked AI to help me punch holes through my research question. My research question initially was how do players that participate in the NFL combine tend to perform in htoir first five seasons of their NFL career. I pointed out that in my data sources the stats ony covered 2012-2025, The files dont share a player id, We can’t tell a player with zero production apart from one we failed to match, Undrafted players have no draft position(Beacuse the players who join the NFL from college can come in 2 ways as Drafeted/UDFA- undrafted free agents), the stats csv also include POST season stats so i would need to look at regular season only

Ai helped me modify how i load the CSV files with a pointer to git lfs files for the actual data and help whether Colab check it has the actual CSV or just a Git LFS pointer. A pointer is a small text file that tells Git LFS where to find the real file. It doesn’t contain any NFL data.