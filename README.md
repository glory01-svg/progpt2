# progpt2
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace RaceDayAPI.Models
{
    public class Organiser
    {
        [Key]
        public int OrganiserId { get; set; }
        [Required]
        [StringLength(100)]
        public string FullName { get; set; }
        [Required]
        [EmailAddress]
        public string Email { get; set; }
        [Required]
        public string Phone { get; set; }
        [Required]
        public string Organisation { get; set; }
        [Required]
        public string PasswordHash { get; set; }
        public string? ProfilePicUrl { get; set; }
        public bool IsVerified { get; set; } = false;
        public DateTime CreatedAt { get; set; } = DateTime.Now;
        public DateTime UpdatedAt { get; set; } = DateTime.Now;

        public ICollection<Event> Events { get; set; }
    }
}      
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using RaceDay.models;

namespace RaceDay.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class EnrollmentController : Controller
    {
      
            private readonly RaceDayDbContext _context;
            public EnrollmentController(RaceDayDbContext context) => _context = context;

            [HttpPost]
            [Authorize(Roles = "Participant")]
            public async Task<IActionResult> Enroll([FromBody] Enrollment enrollment)
            {
                var userId = int.Parse(User.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value);
                enrollment.UserId = userId;
                enrollment.EntryDate = DateTime.Now;

                
                var eventCategory = await _context.EventCategories.FindAsync(enrollment.EventCategoryId);
                if (eventCategory == null) return NotFound("Category not found");
                if (eventCategory.EntriesCount >= eventCategory.Capacity) return BadRequest("Category is full");

            
                eventCategory.EntriesCount++;
                await _context.SaveChangesAsync();

                return Ok(new { message = "Enrolled successfully", entryId = enrollment.EntryId });
            }

            [HttpGet("event/{eventId}")]
            [Authorize(Roles = "Organiser")]
            public async Task<IActionResult> GetEnrollmentsForEvent(int eventId)
            {
                var enrollments = await _context.Enrollment
                    .(e => e.AppUser)
                    .Include(e => e.EventCategory)
                    .Where(e => e.EventCategory.EventId == eventId)
                    .ToListAsync();
                return Ok(enrollments);
            }
        }
    }
using System.ComponentModel.DataAnnotations;

namespace RaceDayAPI.Models
{
    public class AppUser
    {
        [Key]
        public int UserId { get; set; }
        [Required]
        public string FullName { get; set; }
        [Required]
        [EmailAddress]
        public string Email { get; set; }
        [Required]
        public string Phone { get; set; }
        [Required]
        public DateTime DateOfBirth { get; set; }
        [Required]
        public string IdNumber { get; set; }
        [Required]
        public string EmergencyContact { get; set; }
        [Required]
        public string EmergencyPhone { get; set; }
        [Required]
        public string PasswordHash { get; set; }
        public string? ProfilePicUrl { get; set; }
        public string PreferredLanguage { get; set; } = "English";
        public bool IsActive { get; set; } = true;
        public DateTime CreatedAt { get; set; } = DateTime.Now;
        public DateTime UpdatedAt { get; set; } = DateTime.Now;

        public ICollection<Enrollment> Enrollments { get; set; }
    }
}            
using System.ComponentModel.DataAnnotations;

namespace RaceDayAPI.Models
{
    public class AppUser
    {
        [Key]
        public int UserId { get; set; }
        [Required]
        public string FullName { get; set; }
        [Required]
        [EmailAddress]
        public string Email { get; set; }
        [Required]
        public string Phone { get; set; }
        [Required]
        public DateTime DateOfBirth { get; set; }
        [Required]
        public string IdNumber { get; set; }
        [Required]
        public string EmergencyContact { get; set; }
        [Required]
        public string EmergencyPhone { get; set; }
        [Required]
        public string PasswordHash { get; set; }
        public string? ProfilePicUrl { get; set; }
        public string PreferredLanguage { get; set; } = "English";
        public bool IsActive { get; set; } = true;
        public DateTime CreatedAt { get; set; } = DateTime.Now;
        public DateTime UpdatedAt { get; set; } = DateTime.Now;

        public ICollection<Enrollment> Enrollments { get; set; }
    }
}
using System.ComponentModel.DataAnnotations;

namespace RaceDayAPI.Models
{
    public class AppUser
    {
        [Key]
        public int UserId { get; set; }
        [Required]
        public string FullName { get; set; }
        [Required]
        [EmailAddress]
        public string Email { get; set; }
        [Required]
        public string Phone { get; set; }
        [Required]
        public DateTime DateOfBirth { get; set; }
        [Required]
        public string IdNumber { get; set; }
        [Required]
        public string EmergencyContact { get; set; }
        [Required]
        public string EmergencyPhone { get; set; }
        [Required]
        public string PasswordHash { get; set; }
        public string? ProfilePicUrl { get; set; }
        public string PreferredLanguage { get; set; } = "English";
        public bool IsActive { get; set; } = true;
        public DateTime CreatedAt { get; set; } = DateTime.Now;
        public DateTime UpdatedAt { get; set; } = DateTime.Now;

        public ICollection<Enrollment> Enrollments { get; set; }
    }
}
https://youtu.be/utMiRTqIP7E?si=I1hSBesaCZ1SGAFI

