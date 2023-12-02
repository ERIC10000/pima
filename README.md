# Pima Backend Code

```
from flask import *

app = Flask(__name__)


@app.route('/', methods=['POST', 'GET'])
def predict():
    if request.method == 'POST':

        import pandas
        data = pandas.read_csv('/home/erickDiabetes/mysite/data/pima.csv')

        matrix = data.values

        X = matrix[:, 0:8]
        Y = matrix[:, 8]

        from sklearn import model_selection
        X_train, X_test, Y_train, Y_test = model_selection.train_test_split(
            X, Y, test_size=0.3, random_state=42)

        from sklearn.ensemble import GradientBoostingClassifier

        model = GradientBoostingClassifier()

        model.fit(X_train, Y_train)

        children = request.form['children']
        glucose = request.form['glucose']
        bp = request.form['bp']
        st = request.form['st']
        insulin = request.form['insulin']
        bmi = request.form['bmi']
        dpf = request.form['dpf']
        age = request.form['age']


        symptoms = [[children, glucose, bp, st, insulin, bmi, float(dpf), age]]
        condition = model.predict(symptoms)

        return render_template('predict.html', outcome=str(condition[0]))

    else:
        return render_template('predict.html')





```
