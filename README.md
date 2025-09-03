from flask import Flask, request, render_template_string

app = Flask(__name__)

posts = []

HTML_TEMPLATE = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Simple Blog</title>
</head>
<body>
    <h1>Simple Blog</h1>
    <form method="post">
        <input type="text" name="title" placeholder="Post Title" required>
        <br><br>
        <textarea name="content" placeholder="Post Content" required></textarea>
        <br><br>
        <button type="submit">Add Post</button>
    </form>
    <h2>All Posts:</h2>
    {% for post in posts %}
        <div style="border:1px solid #ccc; padding:10px; margin:10px 0;">
            <h3>{{ post.title }}</h3>
            <p>{{ post.content }}</p>
        </div>
    {% endfor %}
</body>
</html>
"""

@app.route("/", methods=["GET", "POST"])
def home():
    if request.method == "POST":
        title = request.form.get("title")
        content = request.form.get("content")
        if title and content:
            posts.append({"title": title, "content": content})
    return render_template_string(HTML_TEMPLATE, posts=posts)

if __name__ == "__main__":
    app.run(debug=True)
