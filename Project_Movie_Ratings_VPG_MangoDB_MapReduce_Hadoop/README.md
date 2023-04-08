# Project Movie Ratings VPG MangoDB MapReduce Hadoop 04.2023

1. This project demonstrates how to use Hadoop with MapReduce in a local mode on Linux Ubuntu  to calculate average movie ratings.
2. Then after preprocessing it computes the similarity between movies based on their genres, tags, or user ratings. The Map function emits pairs of movie IDs and their associated features, while the Reduce function computes a similarity score between the pairs.
3. Then it shows how to set up MongoDB database
4. Creates Reinforcement Learning environment
5. Implements the VPG agent using PyTorch and Numpy
6. Evaluates the VPG agent
7. Compute performance metrics using MapReduce
8. Stores and tracks the agent's performance in MongoDB:

## Installing JDK20, Hadoop and needed dependencies
1. Download JDK20 or newer from Oracle.com:
wget https://download.oracle.com/java/20/latest/jdk-20_linux-x64_bin.tar.gz
2.Extract it:
tar -xvf jdk-20_linux-x64_bin.tar.gz
3.Move it to desired location
sudo mkdir -p /usr/lib/jvm
sudo mv jdk-20 /usr/lib/jvm
4.Set JAVA_HOME environment variable and update PATH:
export JAVA_HOME=/usr/lib/jvm/jdk-20
export PATH=$PATH:$JAVA_HOME/bin
5.Apply the changes by running:
source .bashrc
6.Verify the Java installation:
java -version

7.Now for "Hadoop 3.3.5", I have found that it is best to follow this guide:
https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html
## Map Reduce python script and data for an example
```
import sys

def mapper():
    for line in sys.stdin:
        line = line.strip()
        userId, movieId, rating, timestamp = line.split(',')
        if movieId.isdigit() and rating.replace('.', '').isdigit():
            print(f"{movieId}\t{rating}")

def reducer():
    current_movieId = None
    sum_ratings = 0
    count_ratings = 0

    for line in sys.stdin:
        line = line.strip()
        movieId, rating = line.split('\t')

        if current_movieId == movieId:
            sum_ratings += float(rating)
            count_ratings += 1
        else:
            if current_movieId:
                avg_rating = sum_ratings / count_ratings
                print(f"{current_movieId}\t{avg_rating}")

            current_movieId = movieId
            sum_ratings = float(rating)
            count_ratings = 1

    # Output the last movie's average rating
    if current_movieId:
        avg_rating = sum_ratings / count_ratings
        print(f"{current_movieId}\t{avg_rating}")

if __name__ == '__main__':
    if sys.argv[1] == 'map':
        mapper()
    elif sys.argv[1] == 'reduce':
        reducer()
```
data is downloaded file rating.csv from https://www.kaggle.com/grouplens/movielens-20m-dataset
## Running the MapReduce Job

1. Copy your Python script to the `scripts` folder, and your data file to the `data` folder. Note that when you copy files to these folders, they become the file itself, not directories containing the files.

2. Execute the MapReduce job with the following command:

```bash
cd $HADOOP_HOME
bin/hadoop jar share/hadoop/tools/lib/hadoop-streaming-*.jar \
  -files /home/y/scripts,/home/y/data \
  -input file:///home/y/data \
  -output file:///home/y/output \
  -mapper "python3 /home/y/scripts map" \
  -reducer "python3 /home/y/scripts reduce" \
```

##Checking the output
take the path to the file in ~/output as file and run simple python script, for my file
```
output_file = "path\to\output\file"
output_data = pd.read_csv(output_file, sep='\t', header=None, names=["MovieID", "AverageRating"])
```

