---
permalink: /blog/
title: "Latest"
author_profile: true
---

<details>
<summary style="cursor: pointer; font-weight: bold;">Kaggle Slayer: Introduction to XGBoost</summary>

<p>
  Every Kaggle competition presents unique obstacles—requiring the sharpest of skills and the most powerful of tools. In this post, I will introduce you to the greatest of weapons—one every data scientist must have in their arsenal: <span 
  style="color:#007BFF;">XGBoost</span>.
</p>

<div style="text-align:center; margin: 20px;">
  <img src="/images/dragon_slayer.png" alt="Dragon Slayer" style="width: 450px;"/>
</div>

<p>
  Short for “Extreme Gradient Boosting,” XGBoost is a highly scalable, efficient decision tree machine learning library that makes use of the gradient boosting framework to provide excellent results in classification and regression tasks. If you're just     
  entering the battlefield of machine learning, this tutorial will teach you the basics of XGBoost and help you begin using it to conquer your next challenge.
</p>

<h3>Foundations</h3>

<p>
  The term “gradient boosting” comes from the concept of “boosting”—improving a single weak model (in this case, a decision tree) by combining it with other weak models to form a collectively strong model.
</p>

<p>
  In gradient boosting, an ensemble of shallow decision trees is iteratively trained, with each tree focusing on correcting the residual errors of the previous model. The final prediction is a weighted sum of all the tree predictions, creating a robust model   from individually weak learners.
</p>

<p>
  If you want to learn more about any of these concepts, I highly recommend checking out the YouTube channel "StatQuest with Josh Starmer." He offers a ton of simple yet thorough videos on machine learning. Here are a few of his playlists that explain the     foundational components of XGBoost:
</p>

<ul>
  <li><a href="https://www.youtube.com/watch?v=_L39rN6gz7Y&list=PLblh5JKOoLUKAtDViTvRGFpphEc24M-QH">Decision Trees</a></li>
  <li><a href="https://www.youtube.com/watch?v=3CC4N4z3GJc&list=PLblh5JKOoLUJjeXUvUE0maghNuY2_5fY6">Gradient Boosting</a></li>
</ul>

<div style="text-align:center;">
  <img src="/images/xgboost.png" alt="XGBoost"/>
</div>

<h3>Why XGBoost?</h3>

<p>
  So what is it that makes XGBoost reign king when it comes to working with tabular data? For the purpose of this post, I have narrowed it down to four key factors:
</p>

<ul>
  <li>
      Efficiency: XGBoost is extremely fast. It uses parallel processing and optimization techniques, allowing it to construct different parts of the trees simultaneously. This significantly expedites the process when compared to traditional gradient boosting         methods, which build each tree one by one. It is also sparse-aware, which just means it only stores non-zero values in order to conserve memory. The method's handling of the data enables it to excel with large datasets, making it a great option for   
      applications in big data.
  </li>
  <li>
      Flexibility: XGBoost is highly adaptable, offering built-in functions for handling missing values and responding well to outliers. It includes tons of tuning parameters, which allows users to customize the model depending on the task and dataset. By creating a simple parameter grid, users can quickly test various combinations of hyperparameters to identify the optimal configuration for their problem.
  </li>
  <li>
      Performance: As XGBoost is a tree-based method, it is capable of identifying complex, non-linear relationships in the data. It also uses regularization parameters (<a href="https://www.youtube.com/watch?v=NGf0voTMlcs">L1</a> and <a   
      href="https://www.youtube.com/watch?v=Q81RR3yKn30">L2</a>)to prevent overfitting, enhancing the model's ability to generalize well to out-of-sample data. These capabilities allow XGBoost to be a consistent, high performer, both in Kaggle competitions   
      and in real-world settings. 
  </li>
  <li>
      Community: Lastly, XGBoost is supported by a massive community, meaning extensive up-to-date documentation, tutorials, and resources are readily available to users. There are implementations of XGBoost in Python, R, Julia, Java, and more, so it is 
      accessible for everyone. It also integrates seamlessly with popular data science libraries, such as sci-kit learn and TensorFlow, which makes it easy for users to incorporate it into their workflows.
  </li>
</ul>

<h3>Applications</h3>

<div style="text-align:center; margin: 20px;">
  <img src="/images/fernando_tatis.png" alt="baseball" style="width: 450px;"/>
</div>

<p>
  XGBoost has been successfully applied to:
</p>

<ul>
    <li>
        Sports: Predicting Match Outcomes, Sports Betting, Play Calling, Personnel Strategy, Draft Decisions, Injury Prediction and Prevention
    </li>
    <li>
        Business: Customer Segmentation, Credit Scoring, Risk Assessment, Sale Forecasting
    </li>
    <li>
        Health: Precision Medicine, Healthcare Cost Prediction, Pharmaceutical Studies, Genomics 
    </li>
</ul>

<h3>Demo</h3>

<p>
    Now, I will show you how easy it is start using XGBoost. Below, I trained a simple XGBoost model and compared to three common methods. 
</p>

<pre style="font-size: 12px; padding: 10px; line-height: 1.2;"><code class="language-python">
# Load the dataset
data = pd.read_csv('insurance.csv')

# Define the features and the target variable
X, y = data.iloc[:, :-1], data.iloc[:, -1]

# Define the categorical and numerical features
categorical_features = ['sex', 'smoker', 'region']
numerical_features = ['age', 'bmi', 'children']

# Preprocess the data
preprocessor = ColumnTransformer(
    transformers=[
        ('num', 'passthrough', numerical_features),
        ('cat', OneHotEncoder(), categorical_features)
    ])

# Split the data into training and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Define the models
models = {
    'Linear Regression': LinearRegression(),
    'Decision Tree': DecisionTreeRegressor(random_state=42),
    'Random Forest': RandomForestRegressor(random_state=42),
    'XGBoost': xgb.XGBRegressor(objective='reg:squarederror', random_state=42)
}

# Define a parameter grid for XGBoost
param_grid = {
    'regressor__n_estimators': [100, 200, 300],
    'regressor__max_depth': [3, 4, 5, 6],
    'regressor__learning_rate': [0.01, 0.05, 0.1],
    'regressor__subsample': [0.6, 0.8, 1.0],
    'regressor__colsample_bytree': [0.6, 0.8, 1.0]
}

# Perform RandomizedSearchCV for XGBoost
xgb_pipeline = Pipeline(steps=[('preprocessor', preprocessor),
                               ('regressor', models['XGBoost'])])

random_search = RandomizedSearchCV(estimator=xgb_pipeline, param_distributions=param_grid,
                                   n_iter=50, scoring='neg_mean_squared_error', cv=3, verbose=1, random_state=42, n_jobs=-1)

random_search.fit(X_train, y_train)

# Get the best model
best_xgb_model = random_search.best_estimator_

# Train and evaluate each model
results = {}
for name, model in models.items():
    if name == 'XGBoost':
        pipeline = best_xgb_model
    else:
        pipeline = Pipeline(steps=[('preprocessor', preprocessor),
                                   ('regressor', model)])
        pipeline.fit(X_train, y_train)
    predictions = pipeline.predict(X_test)
    rmse = np.sqrt(mean_squared_error(y_test, predictions))
    results[name] = rmse

# Print the results
for name, rmse in results.items():
    print(f'{name} - RMSE: {rmse}')
</code></pre>

<div style="text-align:center; margin: 20px;">
  <img src="/images/xgboost-demo 2.png" alt="Demo Results" style="width: 450px;"/>
</div>

<p>
    Voila! XGBoost has the lowest RMSE, meaning that its predicted values were closest to the true values in data.
</p>

<h3>Conclusion</h3>

<p>
    With this brief introduction, I hope you have started to appreciate the power of XGBoost. I have avoided diving into the math behind the method here, but if you're interested, it’s fascinating to see what’s happening under the hood.
</p>

<p>
    If you are interested, you can find more information <a href="https://www.geeksforgeeks.org/xgboost/">here</a>.
</p>
  
<p>
    Now that you have some basic information, I would encourage you to try it yourself. With a bit of practice, I am confident you will become a   
    master of XGBoost, slaying even the mightiest of Kaggle competitions in no time. 
</p>
</details>






