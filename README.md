using Microsoft.AspNetCore.Mvc;
using System.ComponentModel.DataAnnotations;
using System.Text.RegularExpressions;
using System.Collections.Generic;
using System.Linq;

namespace SeuProjeto.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class AlunoController : ControllerBase
    {
        private static List<Aluno> alunos = new List<Aluno>();

        public class Aluno
        {
            [Required, MinLength(3)]
            public string Nome { get; set; }

            [Required, RegularExpression(@"^RA\d{6}$", ErrorMessage = "RA deve começar com 'RA' seguido de 6 números.")]
            public string Ra { get; set; }

            [Required, EmailAddress]
            public string Email { get; set; }

            [Required, StringLength(11, MinimumLength = 11, ErrorMessage = "CPF deve ter 11 dígitos.")]
            public string Cpf { get; set; }

            public bool Ativo { get; set; }
        }

        [HttpPost("adicionar")]
        public IActionResult Adicionar([FromBody] Aluno aluno)
        {
            if (!ModelState.IsValid)
                return BadRequest(ModelState);

            alunos.Add(aluno);
            return Ok(aluno);
        }
    }
}
