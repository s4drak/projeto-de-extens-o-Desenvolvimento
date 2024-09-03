# projeto-de-extens-o-Desenvolvimento
from flask import Flask, render_template, request, redirect, url_for
from config import Config

app = Flask(__name__)
app.config.from_object(Config)

# Simulação de um banco de dados com uma lista
events = []

@app.route('/')
def index():
    return render_template('index.html', events=events)

@app.route('/add_event', methods=['GET', 'POST'])
def add_event():
    if request.method == 'POST':
        event_name = request.form['name']
        event_date = request.form['date']
        events.append({'name': event_name, 'date': event_date})
        return redirect(url_for('index'))
    return render_template('add_event.html')

@app.route('/delete_event/<int:index>')
def delete_event(index):
    if 0 <= index < len(events):
        events.pop(index)
    return redirect(url_for('index'))

if __name__ == '__main__':
    app.run(debug=True)

class Config:
    SECRET_KEY = 'your_secret_key_here'

<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Event Management</title>
</head>
<body>
    <h1>Event Management</h1>
    <a href="{{ url_for('add_event') }}">Add New Event</a>
    <ul>
        {% for event in events %}
            <li>{{ event.name }} - {{ event.date }}
                <a href="{{ url_for('delete_event', index=loop.index0) }}">Delete</a>
            </li>
        {% endfor %}
    </ul>
</body>
</html>

<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Add Event</title>
</head>
<body>
    <h1>Add Event</h1>
    <form method="post">
        <label for="name">Event Name:</label>
        <input type="text" id="name" name="name" required>
        <br>
        <label for="date">Event Date:</label>
        <input type="date" id="date" name="date" required>
        <br>
        <button type="submit">Add Event</button>
    </form>
    <a href="{{ url_for('index') }}">Back to Home</a>
</body>
</html>
