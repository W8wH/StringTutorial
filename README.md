# StringTutorial
using dnlib.DotNet;
using dnlib.DotNet.Emit;
using dnlib.DotNet.Writer;
    class Program
    {
        static void Main(string[] args)
        {
            ModuleDefMD module = ModuleDefMD.Load(args[0]); // هنا يسوي لود للبرنامج عشان يدخل على التشفير
            String(module); // < التشغيل
            module.Write("ScoldString.exe", new ModuleWriterOptions(module)
            {
                PEHeadersOptions = { NumberOfRvaAndSizes = 13 },
                Logger = DummyLogger.NoThrowInstance
            }); // < الحفظ
        }
        public static void String(ModuleDefMD module ) // هنا يستدعي البرنامج لن بلعقل وشلون تبي تشفره بدون مايكون في برنامج؟ 
        {
            foreach (TypeDef type in module.GetTypes())  // هنا يستدعي الtypes
            {
                foreach (MethodDef method in type.Methods) // هنا يستدعي الميثود Methods
                {
                    for (var i = 0; i < method.Body.Instructions.Count; i++)
                    {
                        if (method.Body.Instructions[i].OpCode == OpCodes.Ldstr) // هنا نحدد اللي نبغاه مثل الحين حنا حددنا السترنق اللي هو ldstr 
                        {
                            var originalSTR = method.Body.Instructions[i].Operand as string; // هنا ياخذ السطر الاصلي
                            method.Body.Instructions[i].Operand = "Scold";  // هنا يحط السطر الجديد
                        }
                    }
                }
            }
        }
    }
    
# Before
![B](https://user-images.githubusercontent.com/67763829/97521839-781bef80-19af-11eb-9cdd-6c701ddeb6cc.PNG)
# After
![A](https://user-images.githubusercontent.com/67763829/97521871-8407b180-19af-11eb-9fb3-9372f5cad324.PNG)
