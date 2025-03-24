To fix the issue with the instructor registration and login functionality, you need to ensure that the necessary **Supabase tables** and **Storage bucket** are properly set up in your Supabase dashboard. Here's a step-by-step guide to help you configure everything correctly:

---

### **1. Create the Required Tables in Supabase**
You need to create the following tables in your Supabase database:
- `instructors`
- `students`
- `announcements`

#### **Step-by-Step:**
1. **Go to the Supabase Dashboard**:
   - Navigate to your project and open the **Table Editor**.

2. **Create the `instructors` Table**:
   - Click **New Table**.
   - Name the table `instructors`.
   - Add the following columns:
     - `id` (UUID, primary key)
     - `email` (text, unique)
     - `name` (text)
     - `occupation` (text)
     - `bio` (text)
     - `url` (text)
     - `image` (text)
     - `interest` (text)
     - `followers` (array of UUIDs, default: `[]`)
     - `created_at` (timestamp with time zone, default: `now()`)

3. **Create the `students` Table**:
   - Click **New Table**.
   - Name the table `students`.
   - Add the following columns:
     - `id` (UUID, primary key)
     - `email` (text, unique)
     - `name` (text)
     - `interest` (text)
     - `following_list` (array of UUIDs, default: `[]`)
     - `created_at` (timestamp with time zone, default: `now()`)

4. **Create the `announcements` Table**:
   - Click **New Table**.
   - Name the table `announcements`.
   - Add the following columns:
     - `id` (bigint, primary key, auto-increment)
     - `author_name` (text)
     - `author_title` (text)
     - `author_id` (UUID)
     - `content` (text)
     - `likes` (array of UUIDs, default: `[]`)
     - `author_image` (text)
     - `created_at` (timestamp with time zone, default: `now()`)

---

### **2. Set Up Access Policies for the Tables**
You need to configure access policies to allow authenticated users to interact with the tables.

#### **Step-by-Step:**
1. **Go to the SQL Editor** in your Supabase Dashboard.

2. **Add Policies for the `instructors` Table**:
   - Allow authenticated users to insert, update, and read:
     ```sql
     -- Enable insert for authenticated users
     create policy "Enable insert for authenticated users"
     on public.instructors
     for insert
     to authenticated
     with check (true);

     -- Enable update for authenticated users
     create policy "Enable update for authenticated users"
     on public.instructors
     for update
     to authenticated
     using (true);

     -- Enable read for all users
     create policy "Enable read for all users"
     on public.instructors
     for select
     to public
     using (true);
     ```

3. **Add Policies for the `students` Table**:
   - Allow authenticated users to insert, update, and read:
     ```sql
     -- Enable insert for authenticated users
     create policy "Enable insert for authenticated users"
     on public.students
     for insert
     to authenticated
     with check (true);

     -- Enable update for authenticated users
     create policy "Enable update for authenticated users"
     on public.students
     for update
     to authenticated
     using (true);

     -- Enable read for authenticated users
     create policy "Enable read for authenticated users"
     on public.students
     for select
     to authenticated
     using (true);
     ```

4. **Add Policies for the `announcements` Table**:
   - Allow authenticated users to insert, update, and read:
     ```sql
     -- Enable insert for authenticated users
     create policy "Enable insert for authenticated users"
     on public.announcements
     for insert
     to authenticated
     with check (true);

     -- Enable update for authenticated users
     create policy "Enable update for authenticated users"
     on public.announcements
     for update
     to authenticated
     using (true);

     -- Enable read for all users
     create policy "Enable read for all users"
     on public.announcements
     for select
     to public
     using (true);
     ```

---

### **3. Create the `headshots` Storage Bucket**
You need to create a bucket in Supabase Storage to store instructor headshot images.

#### **Step-by-Step:**
1. **Go to the Storage Section** in your Supabase Dashboard.
2. **Create a New Bucket**:
   - Click **New Bucket**.
   - Name the bucket `headshots`.
   - Set the bucket to **Public** (if you want the images to be publicly accessible) or **Private** (if you want to restrict access).
3. **Set Access Policies for the Bucket**:
   - If the bucket is **Private**, add the following policy to allow authenticated users to upload and read files:
     ```sql
     -- Allow authenticated users to upload files
     create policy "Allow upload for authenticated users"
     on storage.objects
     for insert
     to authenticated
     with check (bucket_id = 'headshots');

     -- Allow authenticated users to read files
     create policy "Allow read for authenticated users"
     on storage.objects
     for select
     to authenticated
     using (bucket_id = 'headshots');
     ```

---

### **4. Verify Your Environment Variables**
Ensure your `.env.local` file contains the correct Supabase credentials:
```plaintext
NEXT_PUBLIC_SUPABASE_URL=https://pwdkzkjnrnktgcvvduss.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InB3ZGt6a2pucm5rdGdjdnZkdXNzIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NDI1NTMzMzcsImV4cCI6MjA1ODEyOTMzN30.WJEgTsVm8rnQizE7ruQ0uOw7z2TeoLAQTIA4RDsiKTc
```

---

### **5. Test the Registration and Login Functionality**
1. **Restart Your Next.js Development Server**:
   - Run `npm run dev` to restart your server.

2. **Register as an Instructor**:
   - Fill out the instructor registration form and upload a headshot image.
   - Check the browser console and server logs for any errors.

3. **Verify the Image Upload**:
   - Go to the **Storage** section in your Supabase Dashboard.
   - Ensure the image was uploaded to the `headshots` bucket.

4. **Check the Database**:
   - Go to the **Table Editor** in your Supabase Dashboard.
   - Verify that the instructor's details were added to the `instructors` table.

---

### **6. Debugging Tips**
- If the registration fails, check the browser console and server logs for error messages.
- Ensure the `headshots` bucket exists and is correctly configured.
- Verify that the environment variables are correctly set in your `.env.local` file.
- If the image upload fails, ensure the file is being correctly passed from the form.

---

By following these steps, you should be able to resolve the issue with the instructor registration and login functionality. Let me know if you encounter any further issues!
