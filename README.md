# Music Streaming Data Pipeline

Welcome to the Music Streaming Data Pipeline project! This advanced system processes and analyzes vast amounts of user listening data to provide personalized recommendations, track trends, and optimize content delivery for a music streaming platform.

## Table of Contents
- [Project Overview](#project-overview)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Example: Generating User Recommendations](#example-generating-user-recommendations)
- [Example: Trend Analysis](#example-trend-analysis)
- [Example: User Listening History Analysis](#example-user-listening-history-analysis)
- [CI/CD with GitHub Actions](#cicd-with-github-actions)
- [License](#license)

## Project Overview

Our Music Streaming Data Pipeline is designed to handle large-scale data processing for a music streaming service. It includes real-time data ingestion, processing, analysis, and recommendation components to enhance user experience and provide valuable insights for content management.

Key features:
- Real-time user activity tracking and processing
- Scalable data processing using Apache Spark
- Machine learning models for personalized music recommendations
- Trend analysis and popular content identification
- Artist and label performance analytics
- Content delivery optimization
- Integration with external music databases (e.g., MusicBrainz)

## Project Structure

```
music-streaming-pipeline/
│
├── src/
│   ├── ingestion/
│   │   ├── kafka_consumer.py
│   │   └── api_ingestion.py
│   ├── processing/
│   │   ├── spark_jobs/
│   │   │   ├── listening_history_processing.py
│   │   │   └── user_behavior_analysis.py
│   │   └── ml_models/
│   │       ├── recommendation_engine.py
│   │       └── genre_classification.py
│   ├── analysis/
│   │   ├── trend_analysis.py
│   │   └── artist_performance.py
│   ├── visualization/
│   │   ├── user_dashboard.py
│   │   └── admin_dashboard.py
│   └── integrations/
│       └── musicbrainz_client.py
│
├── tests/
│   ├── unit/
│   └── integration/
│
├── config/
│   ├── kafka_config.yml
│   ├── spark_config.yml
│   ├── ml_config.yml
│   └── api_config.yml
│
├── scripts/
│   ├── setup.sh
│   └── run_pipeline.sh
│
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── cd.yml
│
├── requirements.txt
├── Dockerfile
├── .gitignore
└── README.md
```

## Installation

1. Clone the repository:
   ```
   git clone https://github.com/your-org/music-streaming-pipeline.git
   cd music-streaming-pipeline
   ```

2. Set up a virtual environment:
   ```
   python -m venv venv
   source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
   ```

3. Install dependencies:
   ```
   pip install -r requirements.txt
   ```

4. Set up configuration files in the `config/` directory.

## Usage

1. Start the data ingestion process:
   ```
   python src/ingestion/kafka_consumer.py
   ```

2. Run Spark processing jobs:
   ```
   spark-submit src/processing/spark_jobs/listening_history_processing.py
   ```

3. Generate recommendations:
   ```
   python src/processing/ml_models/recommendation_engine.py
   ```

4. Run trend analysis:
   ```
   python src/analysis/trend_analysis.py
   ```

For more detailed instructions, refer to the documentation in each module.

## Example: Generating User Recommendations

Here's a simple example of how to use the recommendation engine to generate personalized song recommendations for a user:

```python
from src.processing.ml_models.recommendation_engine import RecommendationEngine

# Initialize the recommendation engine
rec_engine = RecommendationEngine(model_path='path/to/trained_model')

# User ID for which we want to generate recommendations
user_id = '12345'

# Get the user's listening history
user_history = rec_engine.get_user_history(user_id)

# Generate recommendations
recommendations = rec_engine.generate_recommendations(user_id, user_history, n=10)

# Print the recommended songs
print(f"Top 10 song recommendations for user {user_id}:")
for i, song in enumerate(recommendations, 1):
    print(f"{i}. {song['title']} by {song['artist']} - {song['confidence']:.2f}")
```

This example demonstrates how to:
1. Initialize the recommendation engine with a pre-trained model
2. Fetch a user's listening history
3. Generate personalized recommendations based on that history
4. Display the top 10 recommended songs with their confidence scores

You can extend this example to integrate recommendations into your music streaming application's user interface or use it for personalized playlist generation.

## Example: Trend Analysis

Here's an example of how to use the trend analysis module to identify rising artists and trending genres:

```python
from src.analysis.trend_analysis import TrendAnalyzer
import matplotlib.pyplot as plt

# Initialize the trend analyzer
trend_analyzer = TrendAnalyzer()

# Get trending artists over the last 30 days
trending_artists = trend_analyzer.get_trending_artists(days=30, limit=10)

# Plot trending artists
plt.figure(figsize=(12, 6))
plt.bar([artist['name'] for artist in trending_artists], [artist['trend_score'] for artist in trending_artists])
plt.title('Top 10 Trending Artists (Last 30 Days)')
plt.xlabel('Artist')
plt.ylabel('Trend Score')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()

# Get trending genres over the last 90 days
trending_genres = trend_analyzer.get_trending_genres(days=90, limit=5)

# Print trending genres
print("Top 5 Trending Genres (Last 90 Days):")
for i, genre in enumerate(trending_genres, 1):
    print(f"{i}. {genre['name']} - Trend Score: {genre['trend_score']:.2f}")
```

This example demonstrates how to:
1. Initialize the trend analyzer
2. Fetch and visualize trending artists over the last 30 days
3. Identify and display trending genres over the last 90 days

You can use this information to curate playlists, feature trending content on the platform's homepage, or provide insights to music labels and artists.

## Example: User Listening History Analysis

Here's an example of how to analyze a user's listening history to provide personalized insights:

```python
from src.processing.spark_jobs.user_behavior_analysis import UserBehaviorAnalyzer
from pyspark.sql import SparkSession

# Initialize Spark session
spark = SparkSession.builder.appName("UserBehaviorAnalysis").getOrCreate()

# Initialize the user behavior analyzer
analyzer = UserBehaviorAnalyzer(spark)

# User ID for analysis
user_id = '12345'

# Get user's listening history for the last 6 months
listening_history = analyzer.get_user_listening_history(user_id, months=6)

# Calculate top genres
top_genres = analyzer.calculate_top_genres(listening_history, limit=5)

# Calculate listening time distribution
time_distribution = analyzer.calculate_listening_time_distribution(listening_history)

# Calculate favorite artists
favorite_artists = analyzer.calculate_favorite_artists(listening_history, limit=3)

# Print insights
print(f"User Insights for user {user_id}:")
print("\nTop 5 Genres:")
for genre, percentage in top_genres:
    print(f"- {genre}: {percentage:.2f}%")

print("\nListening Time Distribution:")
for part_of_day, percentage in time_distribution.items():
    print(f"- {part_of_day}: {percentage:.2f}%")

print("\nTop 3 Favorite Artists:")
for artist, play_count in favorite_artists:
    print(f"- {artist}: {play_count} plays")

# Clean up
spark.stop()
```

This example shows how to:
1. Initialize a Spark session and the user behavior analyzer
2. Fetch a user's listening history for the past 6 months
3. Calculate and display the user's top genres
4. Analyze the user's listening time distribution throughout the day
5. Identify the user's favorite artists based on play count

These insights can be used to create personalized user experiences, such as custom daily mixes, time-based recommendations, or artist spotlight features.

## CI/CD with GitHub Actions

We use GitHub Actions for continuous integration and deployment. Our workflow includes:

1. **Continuous Integration (CI)**
   - Triggered on every push and pull request to the `main` branch
   - Runs unit and integration tests
   - Performs code linting and style checks
   - Builds Docker images

2. **Continuous Deployment (CD)**
   - Triggered on successful merges to the `main` branch
   - Deploys the application to staging environment
   - Runs smoke tests
   - If successful, promotes to production

To view and modify these workflows, check the `.github/workflows/` directory.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
