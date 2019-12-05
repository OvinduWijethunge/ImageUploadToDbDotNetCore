# ImageUploadToDbDotNetCore
 just a image path upload to db and image save in webroot 
 
 
  public IHostingEnvironment _hostingEnv { get; }

        public HomeController(IHostingEnvironment hostingEnv)  //constructer 
        {
            _hostingEnv = hostingEnv;
            // _context = context;
        }
 
  [HttpPost]
        public async Task<string> UploadFile(IFormFile file)
        {
            if (file != null)
            {
                //upload files to wwwroot
                var fileName = Path.GetFileName(file.FileName);
               // string WebRootPath = null;
                // var filePath = Path.Combine(WebRootPath, "Uploads", fileName);
                var filePath = Path.Combine(_hostingEnv.WebRootPath, "images\\photo", fileName);



                using (var fileSteam = new FileStream(filePath, FileMode.Create))
                {
                    await file.CopyToAsync(fileSteam);
                }
                //your logic to save filePath to database, for example

                //Projects projects = new Projects();
                //  projects.Name = projectsVM.Name;
                // projects.FilePath = filePath;
                using (var context = new imgdbContext())
                {
                    Image img = new Image();
                    img.Path = filePath;
                    context.Add(img);
                    context.SaveChanges();
                }
                 
            }
