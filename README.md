```
using System;
using System.Collections.Generic;

namespace network
{
    
    struct Comment
    {
        public string AuthorNickname;
        public string Text;

        public Comment(string author, string text)
        {
            AuthorNickname = author;
            Text = text;
        }
    }

    
    class Post
    {
        public DateTime CreatedAt;
        public string Text;
        public int Likes;
        public List<Comment> Comments;

        public Post(string text, DateTime date)
        {
            Text = text;
            CreatedAt = date;
            Likes = 0;
            Comments = new List<Comment>();
        }
    }

    
    class User
    {
        public string Name;
        public string Email;
        public string Nickname;
        public List<Post> Posts;

        public User(string name, string email, string nickname)
        {
            Name = name;
            Email = email;
            Nickname = nickname;
            Posts = new List<Post>();
        }

        
        public void CreatePost(string text)
        {
            Post newPost = new Post(text, DateTime.Now);
            Posts.Add(newPost);
            Console.WriteLine($"[{Nickname}] створив новий пост.");
        }

        
        public void DeletePost(int postIndex)
        {
            if (postIndex >= 0 && postIndex < Posts.Count)
            {
                Posts.RemoveAt(postIndex);
                Console.WriteLine($"[{Nickname}] видалив пост #{postIndex + 1}.");
            }
            else
            {
                Console.WriteLine("Помилка: такого поста не існує.");
            }
        }

        
        public void LikePost(User otherUser, int postIndex)
        {
            if (postIndex >= 0 && postIndex < otherUser.Posts.Count)
            {
                otherUser.Posts[postIndex].Likes++;
                Console.WriteLine($"[{Nickname}] лайкнув пост користувача {otherUser.Nickname}.");
            }
        }

        
        public void CommentPost(User otherUser, int postIndex, string commentText)
        {
            if (postIndex >= 0 && postIndex < otherUser.Posts.Count)
            {
                Comment newComment = new Comment(this.Nickname, commentText);
                otherUser.Posts[postIndex].Comments.Add(newComment);
                Console.WriteLine($"[{Nickname}] прокоментував пост користувача {otherUser.Nickname}.");
            }
        }

        
        public void ShowLast5Posts(User otherUser)
        {
            Console.WriteLine($"\n=== Останні пости користувача {otherUser.Name} ({otherUser.Nickname}) ===");
            
            int totalPosts = otherUser.Posts.Count;
            if (totalPosts == 0)
            {
                Console.WriteLine("У користувача ще немає постів.");
                return;
            }

            
            int startIndex = totalPosts - 5;
            if (startIndex < 0)
            {
                startIndex = 0;
            }

            for (int i = startIndex; i < totalPosts; i++)
            {
                Post p = otherUser.Posts[i];
                Console.WriteLine($"Пост #{i + 1} | Дата: {p.CreatedAt}");
                Console.WriteLine($"Текст: {p.Text}");
                Console.WriteLine($"Лайків: {p.Likes}");
                Console.WriteLine("Коментарі:");
                
                if (p.Comments.Count == 0)
                {
                    Console.WriteLine("  (немає коментарів)");
                }
                else
                {
                    for (int j = 0; j < p.Comments.Count; j++)
                    {
                        Console.WriteLine($"  - {p.Comments[j].AuthorNickname}: {p.Comments[j].Text}");
                    }
                }
                Console.WriteLine("-----------------------------------");
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.OutputEncoding = System.Text.Encoding.UTF8;

            
            User user1 = new User("Олександр", "alex@gmail.com", "@alex_dev");
            User user2 = new User("Марія", "maria@ukr.net", "@mari_art");

            // Марія створює 6 постів, щоб перевірити вивід останніх 5
            user2.CreatePost("Мій перший пост у цій соцмережі!");
            user2.CreatePost("Сьогодні гарна погода для прогулянки.");
            user2.CreatePost("Вчу програмування на C#, поки все зрозуміло.");
            user2.CreatePost("Хто знає гарні курси з дизайну?");
            user2.CreatePost("Випила смачну каву зранку.");
            user2.CreatePost("Мій шостий пост! Перший не має показатися в останніх 5.");

            Console.WriteLine();

            
            user1.LikePost(user2, 5);
            user1.LikePost(user2, 4);
            user1.CommentPost(user2, 5, "Крутий пост, так тримати!");
            user1.CommentPost(user2, 2, "C# топ мова!");

            
            user1.ShowLast5Posts(user2);

            
            user2.DeletePost(4);
            user1.ShowLast5Posts(user2);

            Console.ReadLine();
        }
    }
}
