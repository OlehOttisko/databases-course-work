# Реалізація інформаційного та програмного забезпечення

## SQL-скрипт для створення на початкового наповнення бази даних

```mysql
-- MySQL Script generated by MySQL Workbench
-- Fri Oct 20 09:55:08 2023
-- Model: New Model    Version: 1.0
-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema db_coursework
-- -----------------------------------------------------
DROP SCHEMA IF EXISTS `db_coursework` ;

-- -----------------------------------------------------
-- Schema db_coursework
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `db_coursework` DEFAULT CHARACTER SET utf8 ;

USE `db_coursework` ;

-- -----------------------------------------------------
-- Table `db_coursework`.`Role`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`Role` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`Role` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(45) NOT NULL,
  `description` VARCHAR(180) NULL DEFAULT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db_coursework`.`Permission`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`Permission` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`Permission` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `action` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db_coursework`.`Project`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`Project` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`Project` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(45) NOT NULL,
  `description` VARCHAR(180) NULL DEFAULT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db_coursework`.`Team`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`Team` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`Team` (
  `id` INT NOT NULL,
  `name` VARCHAR(100) NULL,
  `motto` VARCHAR(255) NULL,
  `Project_id` INT NOT NULL,
  INDEX `fk_Team_Project1_idx` (`Project_id` ASC) VISIBLE,
  PRIMARY KEY (`id`),
  CONSTRAINT `fk_Team_Project1`
    FOREIGN KEY (`Project_id`)
    REFERENCES `db_coursework`.`Project` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db_coursework`.`User`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`User` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`User` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `nickname` VARCHAR(45) NOT NULL,
  `email` VARCHAR(45) NOT NULL,
  `password` VARCHAR(45) NOT NULL,
  `photo` VARCHAR(255) NULL DEFAULT NULL,
  `is_banned` TINYINT NOT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db_coursework`.`Collaborator`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`Collaborator` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`Collaborator` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `Role_id` INT NOT NULL,
  `User_id` INT NOT NULL,
  `Team_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_Member_Role1_idx` (`Role_id` ASC) VISIBLE,
  INDEX `fk_Member_User1_idx` (`User_id` ASC) VISIBLE,
  INDEX `fk_Collaborator_Team1_idx` (`Team_id` ASC) VISIBLE,
  CONSTRAINT `fk_Member_Role1`
    FOREIGN KEY (`Role_id`)
    REFERENCES `db_coursework`.`Role` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Member_User1`
    FOREIGN KEY (`User_id`)
    REFERENCES `db_coursework`.`User` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Collaborator_Team1`
    FOREIGN KEY (`Team_id`)
    REFERENCES `db_coursework`.`Team` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db_coursework`.`Sprint`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`Sprint` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`Sprint` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(45) NOT NULL,
  `goal` VARCHAR(100) NOT NULL,
  `startdate` DATETIME NULL DEFAULT NULL,
  `enddate` DATETIME NULL DEFAULT NULL,
  `Project_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_Sprint_Project1_idx` (`Project_id` ASC) VISIBLE,
  CONSTRAINT `fk_Sprint_Project1`
    FOREIGN KEY (`Project_id`)
    REFERENCES `db_coursework`.`Project` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db_coursework`.`Tag`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`Tag` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`Tag` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(45) NOT NULL,
  `description` VARCHAR(180) NULL DEFAULT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db_coursework`.`Task`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`Task` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`Task` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(45) NOT NULL,
  `description` VARCHAR(180) NULL DEFAULT NULL,
  `deadline` DATETIME NULL DEFAULT NULL,
  `creation_date` DATETIME NOT NULL,
  `Sprint_id` INT NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_Task_Sprint1_idx` (`Sprint_id` ASC) VISIBLE,
  CONSTRAINT `fk_Task_Sprint1`
    FOREIGN KEY (`Sprint_id`)
    REFERENCES `db_coursework`.`Sprint` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db_coursework`.`Assignment`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`Assignment` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`Assignment` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `datatime` DATETIME NULL DEFAULT NULL,
  `Collaborator_id` INT NOT NULL,
  `Task_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_Assignment_Member1_idx` (`Collaborator_id` ASC) VISIBLE,
  INDEX `fk_Assignment_Task1_idx` (`Task_id` ASC) VISIBLE,
  CONSTRAINT `fk_Assignment_Member1`
    FOREIGN KEY (`Collaborator_id`)
    REFERENCES `db_coursework`.`Collaborator` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Assignment_Task1`
    FOREIGN KEY (`Task_id`)
    REFERENCES `db_coursework`.`Task` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db_coursework`.`Task_comment`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`Task_comment` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`Task_comment` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `subject` VARCHAR(45) NOT NULL,
  `text` VARCHAR(45) NOT NULL,
  `datetime` DATETIME NULL DEFAULT NULL,
  `Author_id` INT NOT NULL,
  `Task_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_Task_comment_Member1_idx` (`Author_id` ASC) VISIBLE,
  INDEX `fk_Task_comment_Task1_idx` (`Task_id` ASC) VISIBLE,
  CONSTRAINT `fk_Task_comment_Member1`
    FOREIGN KEY (`Author_id`)
    REFERENCES `db_coursework`.`Collaborator` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Task_comment_Task1`
    FOREIGN KEY (`Task_id`)
    REFERENCES `db_coursework`.`Task` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db_coursework`.`Grant`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`Grant` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`Grant` (
  `Role_id` INT NOT NULL,
  `Permission_id` INT NOT NULL,
  INDEX `fk_Grant_Role_idx` (`Role_id` ASC) VISIBLE,
  PRIMARY KEY (`Role_id`, `Permission_id`),
  INDEX `fk_Grant_Permission2_idx` (`Permission_id` ASC) VISIBLE,
  CONSTRAINT `fk_Grant_Role0`
    FOREIGN KEY (`Role_id`)
    REFERENCES `db_coursework`.`Role` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Grant_Permission2`
    FOREIGN KEY (`Permission_id`)
    REFERENCES `db_coursework`.`Permission` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db_coursework`.`Label`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`Label` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`Label` (
  `Task_id` INT NOT NULL,
  `Tag_id` INT NOT NULL,
  PRIMARY KEY (`Task_id`, `Tag_id`),
  INDEX `fk_Label_Tag1_idx` (`Tag_id` ASC) VISIBLE,
  CONSTRAINT `fk_Label_Task1`
    FOREIGN KEY (`Task_id`)
    REFERENCES `db_coursework`.`Task` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Label_Tag1`
    FOREIGN KEY (`Tag_id`)
    REFERENCES `db_coursework`.`Tag` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `db_coursework`.`Action`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `db_coursework`.`Action` ;

CREATE TABLE IF NOT EXISTS `db_coursework`.`Action` (
  `date` DATETIME NULL DEFAULT NULL,
  `Sprint_id` INT NOT NULL,
  `Task_id` INT NOT NULL,
  `Assignment_id` INT NOT NULL,
  `Collaborator_id` INT NOT NULL,
  PRIMARY KEY (`Sprint_id`, `Task_id`, `Assignment_id`, `Collaborator_id`),
  INDEX `fk_Action_Task1_idx` (`Task_id` ASC) VISIBLE,
  INDEX `fk_Action_Assignment1_idx` (`Assignment_id` ASC) VISIBLE,
  INDEX `fk_Action_Collaborator1_idx` (`Collaborator_id` ASC) VISIBLE,
  CONSTRAINT `fk_Action_Sprint1`
    FOREIGN KEY (`Sprint_id`)
    REFERENCES `db_coursework`.`Sprint` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Action_Task1`
    FOREIGN KEY (`Task_id`)
    REFERENCES `db_coursework`.`Task` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Action_Assignment1`
    FOREIGN KEY (`Assignment_id`)
    REFERENCES `db_coursework`.`Assignment` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Action_Collaborator1`
    FOREIGN KEY (`Collaborator_id`)
    REFERENCES `db_coursework`.`Collaborator` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;


-- Додавання тестових даних




-- Початок транзакції
START TRANSACTION;

-- Додавання даних в таблицю `db_coursework`.`Permission`
INSERT INTO `db_coursework`.`Permission` (`action`)
VALUES
    -- collaborator
    ('EditUser'),
    ('CreateTask'),
    ('EditTask'),
    ('DeleteTask'),
    ('FilterTask'),
    ('CommentTask'),
    -- teamlead
    ('CreateProject'),
    ('DeleteProject'),
    ('CreateSprint'),
    ('FinishSprint'),
    ('AddMember'),
    ('DeleteMember'),
    -- admin
    ('UserSupport'),
    ('BanUser'),
    ('UnBanUser');


-- Додавання даних в таблицю `db_coursework`.`Role`
INSERT INTO `db_coursework`.`Role` (`name`, `description`)
VALUES
    ('Administrator', 'Administrator role'),
    ('Team-lead', 'Team-lead role'),
    ('Collaborator', 'Developer role');

-- Додавання даних в таблицю `db_coursework`.`Grant`
INSERT INTO `db_coursework`.`Grant` (`Role_id`, `Permission_id`)
VALUES
    (1, 1),
    (1, 2),
    (1, 3),
    (1, 4),
    (1, 5),
    (1, 6),
    (1, 7),
    (1, 8),
    (1, 9),
    (1, 10),
    (1, 11),
    (1, 12),
    (1, 13),
    (1, 14),
    (1, 15),

    (2, 1),
    (2, 2),
    (2, 3),
    (2, 4),
    (2, 5),
    (2, 6),
    (2, 7),
    (2, 8),
    (2, 9),
    (2, 10),
    (2, 11),
    (2, 12),

    (3, 1),
    (3, 2),
    (3, 3),
    (3, 4),
    (3, 5),
    (3, 6);


-- Додавання даних в таблицю `db_coursework`.`Project`
INSERT INTO `db_coursework`.`Project` (`name`, `description`)
VALUES
    ('Project 1', 'Description for Project 1'),
    ('Project 2', 'Description for Project 2');

-- Додавання даних в таблицю `db_coursework`.`Team`
INSERT INTO `db_coursework`.`Team` (`id`, `name`, `motto`, `Project_id`)
VALUES
    (1, 'Team 1', 'Motto for Team 1', 1),
    (2, 'Team 2', 'Motto for Team 2', 2);

-- Додавання даних в таблицю `db_coursework`.`User`
INSERT INTO `db_coursework`.`User` (`nickname`, `email`, `password`, `photo`, `is_banned`)
VALUES
    ('User1', 'user1@example.com', 'password1', 'link.com/photo', 0),
    ('User2', 'user2@example.com', 'password2', 'link.com/photo', 0);

-- Додавання даних в таблицю `db_coursework`.`Collaborator`
INSERT INTO `db_coursework`.`Collaborator` (`Role_id`, `User_id`, `Team_id`)
VALUES
    (1, 1, 1),  -- Admin User 1 in Team 1
    (2, 2, 2);  -- Manager User 2 in Team 2

-- Додавання тестових даних в таблицю `db_coursework.Sprint`
INSERT INTO `db_coursework`.`Sprint` (`name`, `goal`, `startdate`, `enddate`, `Project_id`)
VALUES
    ('Sprint 1', 'Complete Task 1', '2023-10-18 10:00:00', '2023-10-22 18:00:00', 1),
    ('Sprint 2', 'Finish Project 2', '2023-10-25 09:00:00', '2023-10-29 17:00:00', 2),
    ('Sprint 3', 'Implement Feature 3', '2023-11-01 08:00:00', '2023-11-05 16:00:00', 1);


-- Додавання даних в таблицю `db_coursework`.`Task`
INSERT INTO `db_coursework`.`Task` (`name`, `description`, `deadline`, `creation_date`, `Sprint_id`)
VALUES
    ('Task 1', 'Description for Task 1', '2023-10-31 12:00:00', NOW(), 1),
    ('Task 2', 'Description for Task 2', '2023-11-15 14:30:00', NOW(), 2),
    ('Task 3', 'Description for Task 3', '2023-11-20 10:00:00', NOW(), 3);

-- Додавання тестових даних в таблицю `db_coursework.Assignment`
INSERT INTO `db_coursework`.`Assignment` (`datatime`, `Collaborator_id`, `Task_id`)
VALUES
    ('2023-10-18 11:30:00', 1, 1),
    ('2023-10-19 14:15:00', 2, 2),
    ('2023-10-20 09:45:00', 1, 3);


COMMIT;

```

## RESTfull сервіс для управління даними (Framework: ASP.NET WebAPI)

Цей сервіс розроблений за допомогою Web API, впровадженим фреймворком платформи .NET - ASP.NET.
Базується на базовій архітектурі MVC (Model-View-Controller), де
- Model (Модель) - надає данні та реагує на команди контролера, змінюючи свій стан.
- View (Вид) - відповідає за відображення даних для клієнта (в даному випадку - умовно, тому що це лише Web API).
- Controller (Контролер) - інтерпретує дії користувача й сповіщає Моделі про необхідність змін станів.

ASP.NET надає програмісту дуже зручний функціонал для реалізації веб-додатків і, в поєднанні з фреймворком EntityFramework та бібліотекою LINQ\
(Language Integrated Query) розробник може легко взаємодіяти з базами даних без написання SQL-скриптів напряму.

## Діаграма класів
<p>
    <img src="./images/image_2023-11-13_10-43-22.png">
</p>


### Моделі

#### Project
```csharp
using System.ComponentModel.DataAnnotations;

namespace Duallo.Models;

public class Project
{
    [Key] public int Id { get; set; }
    [MaxLength(45)]public string Name { get; set; }
    [MaxLength(180)]public string Description { get; set; }
}
```

#### Team
```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using Microsoft.AspNetCore.Mvc.ModelBinding.Validation;

namespace Duallo.Models;

public class Team
{
    [Key]public int Id { get; set; }
    [MaxLength(100)]public string Name { get; set; }
    [MaxLength(255)]public string Motto { get; set; }
    public int ProjectId { get; set; }
    [ForeignKey("ProjectId"), ValidateNever]public Project Project { get; set; }
}
```

### Контролери

#### ProjectController
```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using Duallo.Models;

namespace Duallo.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class ProjectController : ControllerBase
    {
        private readonly ApplicationDbContext _context;

        public ProjectController(ApplicationDbContext context)
        {
            _context = context;
        }

        [HttpGet]
        public async Task<ActionResult<IEnumerable<Project>>> GetProjects() => _context.Projects is not null
            ? await _context.Projects.ToListAsync()
            : NotFound();

        [HttpGet("{id}")]
        public async Task<ActionResult<Project>> GetProject(int id)
        {
            if (_context.Projects is null) return NotFound();
            var project = await _context.Projects.FindAsync(id);

            return project is not null ? project : NotFound();
        }

        [HttpPut("{id}")]
        public async Task<IActionResult> PutProject(int id, Project project)
        {
            if (id != project.Id) return BadRequest();
            
            _context.Entry(project).State = EntityState.Modified;

            try
            {
                await _context.SaveChangesAsync();
            }
            catch (DbUpdateConcurrencyException)
            {
                if (!ProjectExists(id))
                    return NotFound();
                else
                    throw;
            }
            return NoContent();
        }

        [HttpPost]
        public async Task<ActionResult<Project>> PostProject(Project project)
        {
            if (_context.Projects is null)
                return Problem("Entity set 'ApplicationDbContext.Projects'  is null.");
            
            _context.Projects.Add(project);
            await _context.SaveChangesAsync();

            return CreatedAtAction("GetProject", new { id = project.Id }, project);
        }

        [HttpDelete("{id}")]
        public async Task<IActionResult> DeleteProject(int id)
        {
            if (_context.Projects is null) return NotFound();
            
            var project = await _context.Projects.FindAsync(id);
            
            if (project is null) return NotFound();
            
            _context.Projects.Remove(project);
            await _context.SaveChangesAsync();

            return NoContent();
        }

        private bool ProjectExists(int id)
        {
            return (_context.Projects?.Any(e => e.Id == id)).GetValueOrDefault();
        }
    }
}
```
#### TeamController
```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using Duallo.Models;

namespace Duallo.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class TeamController : ControllerBase
    {
        private readonly ApplicationDbContext _context;

        public TeamController(ApplicationDbContext context) => _context = context;

        [HttpGet]
        public async Task<ActionResult<IEnumerable<Team>>> GetTeams()
        {
            return _context.Teams is not null
                ? await _context.Teams.Include(c => c.Project).ToListAsync()
                : NotFound();
        }

        [HttpGet("{id}")]
        public async Task<ActionResult<Team>> GetTeam(int id)
        {
            if (_context.Teams is null) return NotFound();

            var team = await _context.Teams
                .Include(c => c.Project).FirstOrDefaultAsync(_ => _.Id == id);

            return team is not null ? team : NotFound();
        }

        [HttpPut("{id}")]
        public async Task<IActionResult> PutTeam(int id, Team team)
        {
            if (id != team.Id) return BadRequest();
            
            _context.Entry(team).State = EntityState.Modified;

            try
            {
                await _context.SaveChangesAsync();
            }
            catch (DbUpdateConcurrencyException)
            {
                if (!TeamExists(id))
                {
                    return NotFound();
                }
            }

            return NoContent();
        }

        [HttpPost]
        public async Task<ActionResult<Team>> PostTeam(Team team)
        {
            if (_context.Teams is null) return Problem("Entity set 'ApplicationDbContext.Teams'  is null.");
            
            _context.Teams.Add(team);
            await _context.SaveChangesAsync();

            return CreatedAtAction("GetTeam", new { id = team.Id }, team);
        }

        [HttpDelete("{id}")]
        public async Task<IActionResult> DeleteTeam(int id)
        {
            if (_context.Teams is null) return NotFound();
            
            var team = await _context.Teams.FindAsync(id);
            if (team is null) return NotFound();
            
            _context.Teams.Remove(team);
            await _context.SaveChangesAsync();

            return NoContent();
        }

        private bool TeamExists(int id) => (_context.Teams?.Any(e => e.Id == id)).GetValueOrDefault();
    }
}
```
### Контекст Бази Даних
Клас для створення таблиць у базі даних за допомогою фреймворку EntityFramework та початкового наповнення бази даних (Seed).

#### ApplicationDbContext
```csharp
using Duallo.Models;
using Microsoft.EntityFrameworkCore;

namespace Duallo;

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
    {
    }

    public DbSet<Project> Projects { get; set; }
    public DbSet<Team> Teams { get; set; }


    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Project>().HasData(
            new Project { Id = 1, Name = "M4", Description = "Most powerful ARM-based chip" },
            new Project { Id = 2, Name = "Google Pixel 9", Description = "The fastest growing smartphone on market" },
            new Project
            {
                Id = 3, Name = "Drone delivery", Description = "The newest and the most convenient delivery ever"
            });
        modelBuilder.Entity<Team>().HasData(
            new Team() { Id = 1, Name = "Apple's main Team", Motto = "Think different", ProjectId = 1 },
            new Team() { Id = 2, Name = "Google's main Team", Motto = "Don't be evil", ProjectId = 2 },
            new Team()
            {
                Id = 3, Name = "Amazon's main Team", Motto = "Work hard, Have fun, Make history", ProjectId = 3
            }
        );
    }
}
```
### Міграційні файли до Бази Даних (Entity Framework)

#### Initial

```csharp
using Microsoft.EntityFrameworkCore.Migrations;
using MySql.EntityFrameworkCore.Metadata;

#nullable disable

#pragma warning disable CA1814 // Prefer jagged arrays over multidimensional

namespace Duallo.Migrations
{
    /// <inheritdoc />
    public partial class Initial : Migration
    {
        /// <inheritdoc />
        protected override void Up(MigrationBuilder migrationBuilder)
        {
            migrationBuilder.AlterDatabase()
                .Annotation("MySQL:Charset", "utf8mb4");

            migrationBuilder.CreateTable(
                name: "Projects",
                columns: table => new
                {
                    Id = table.Column<int>(type: "int", nullable: false)
                        .Annotation("MySQL:ValueGenerationStrategy", MySQLValueGenerationStrategy.IdentityColumn),
                    Name = table.Column<string>(type: "varchar(45)", maxLength: 45, nullable: false),
                    Description = table.Column<string>(type: "varchar(180)", maxLength: 180, nullable: false)
                },
                constraints: table =>
                {
                    table.PrimaryKey("PK_Projects", x => x.Id);
                })
                .Annotation("MySQL:Charset", "utf8mb4");

            migrationBuilder.CreateTable(
                name: "Teams",
                columns: table => new
                {
                    Id = table.Column<int>(type: "int", nullable: false)
                        .Annotation("MySQL:ValueGenerationStrategy", MySQLValueGenerationStrategy.IdentityColumn),
                    Name = table.Column<string>(type: "varchar(100)", maxLength: 100, nullable: false),
                    Motto = table.Column<string>(type: "varchar(255)", maxLength: 255, nullable: false),
                    ProjectId = table.Column<int>(type: "int", nullable: false)
                },
                constraints: table =>
                {
                    table.PrimaryKey("PK_Teams", x => x.Id);
                    table.ForeignKey(
                        name: "FK_Teams_Projects_ProjectId",
                        column: x => x.ProjectId,
                        principalTable: "Projects",
                        principalColumn: "Id",
                        onDelete: ReferentialAction.Cascade);
                })
                .Annotation("MySQL:Charset", "utf8mb4");

            migrationBuilder.InsertData(
                table: "Projects",
                columns: new[] { "Id", "Description", "Name" },
                values: new object[,]
                {
                    { 1, "Most powerful ARM-based chip", "M4" },
                    { 2, "The fastest growing smartphone on market", "Google Pixel 9" },
                    { 3, "The newest and the most convenient delivery ever", "Drone delivery" }
                });

            migrationBuilder.InsertData(
                table: "Teams",
                columns: new[] { "Id", "Motto", "Name", "ProjectId" },
                values: new object[,]
                {
                    { 1, "Think different", "Apple's main Team", 1 },
                    { 2, "Don't be evil", "Google's main Team", 2 },
                    { 3, "Work hard, Have fun, Make history", "Amazon's main Team", 3 }
                });

            migrationBuilder.CreateIndex(
                name: "IX_Teams_ProjectId",
                table: "Teams",
                column: "ProjectId");
        }

        /// <inheritdoc />
        protected override void Down(MigrationBuilder migrationBuilder)
        {
            migrationBuilder.DropTable(
                name: "Teams");

            migrationBuilder.DropTable(
                name: "Projects");
        }
    }
}
```

#### ApplicationDbContextModelSnapshot
```csharp
// <auto-generated />
using Duallo;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;
using Microsoft.EntityFrameworkCore.Storage.ValueConversion;

#nullable disable

namespace Duallo.Migrations
{
    [DbContext(typeof(ApplicationDbContext))]
    partial class ApplicationDbContextModelSnapshot : ModelSnapshot
    {
        protected override void BuildModel(ModelBuilder modelBuilder)
        {
#pragma warning disable 612, 618
            modelBuilder
                .HasAnnotation("ProductVersion", "7.0.13")
                .HasAnnotation("Relational:MaxIdentifierLength", 64);

            modelBuilder.Entity("Duallo.Models.Project", b =>
                {
                    b.Property<int>("Id")
                        .ValueGeneratedOnAdd()
                        .HasColumnType("int");

                    b.Property<string>("Description")
                        .IsRequired()
                        .HasMaxLength(180)
                        .HasColumnType("varchar(180)");

                    b.Property<string>("Name")
                        .IsRequired()
                        .HasMaxLength(45)
                        .HasColumnType("varchar(45)");

                    b.HasKey("Id");

                    b.ToTable("Projects");

                    b.HasData(
                        new
                        {
                            Id = 1,
                            Description = "Most powerful ARM-based chip",
                            Name = "M4"
                        },
                        new
                        {
                            Id = 2,
                            Description = "The fastest growing smartphone on market",
                            Name = "Google Pixel 9"
                        },
                        new
                        {
                            Id = 3,
                            Description = "The newest and the most convenient delivery ever",
                            Name = "Drone delivery"
                        });
                });

            modelBuilder.Entity("Duallo.Models.Team", b =>
                {
                    b.Property<int>("Id")
                        .ValueGeneratedOnAdd()
                        .HasColumnType("int");

                    b.Property<string>("Motto")
                        .IsRequired()
                        .HasMaxLength(255)
                        .HasColumnType("varchar(255)");

                    b.Property<string>("Name")
                        .IsRequired()
                        .HasMaxLength(100)
                        .HasColumnType("varchar(100)");

                    b.Property<int>("ProjectId")
                        .HasColumnType("int");

                    b.HasKey("Id");

                    b.HasIndex("ProjectId");

                    b.ToTable("Teams");

                    b.HasData(
                        new
                        {
                            Id = 1,
                            Motto = "Think different",
                            Name = "Apple's main Team",
                            ProjectId = 1
                        },
                        new
                        {
                            Id = 2,
                            Motto = "Don't be evil",
                            Name = "Google's main Team",
                            ProjectId = 2
                        },
                        new
                        {
                            Id = 3,
                            Motto = "Work hard, Have fun, Make history",
                            Name = "Amazon's main Team",
                            ProjectId = 3
                        });
                });

            modelBuilder.Entity("Duallo.Models.Team", b =>
                {
                    b.HasOne("Duallo.Models.Project", "Project")
                        .WithMany()
                        .HasForeignKey("ProjectId")
                        .OnDelete(DeleteBehavior.Cascade)
                        .IsRequired();

                    b.Navigation("Project");
                });
#pragma warning restore 612, 618
        }
    }
}
```

### Файл запуску програми 
#### Program
```csharp
using Duallo;
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

builder.Services.AddDbContext<ApplicationDbContext>(options => options.UseMySQL(
    builder.Configuration.GetConnectionString("DefaultConnection")));
var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

app.UseAuthorization();

app.MapControllers();

app.Run();
```
