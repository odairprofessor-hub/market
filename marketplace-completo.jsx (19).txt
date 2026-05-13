import React, { useState, useEffect, useRef } from 'react';
import { Send, X, LogOut, Plus, LogIn } from 'lucide-react';

// Estilos Globais Futurísticos
const globalStyles = `
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800&family=Inter:wght@400;500;600;700&display=swap');
  
  * {
    font-family: 'Poppins', sans-serif;
  }
  
  body {
    background: linear-gradient(135deg, #0f0f23 0%, #1a1a3e 50%, #0f2744 100%);
    min-height: 100vh;
  }
`;

const Marketplace = () => {
  const [usuarioLogado, setUsuarioLogado] = useState(JSON.parse(localStorage.getItem('usuarioLogado')));
  const [usuarioIP, setUsuarioIP] = useState(null);
  const [tela, setTela] = useState(usuarioLogado ? 'inicio' : 'inicio');
  
  const [usuarios, setUsuarios] = useState(JSON.parse(localStorage.getItem('usuarios')) || {
    'teste@test.com': { 
      id: 'teste123', 
      nome: '🧪 Teste Completo', 
      email: 'teste@test.com', 
      senha: '123', 
      cidade: 'Santa Rosa', 
      tipo: 'vendedor-comprador',
      descricao: '👤 Use esta conta para testar TODAS as funcionalidades!'
    },
    'vendedor@test.com': { id: 'v1', nome: 'João Silva', email: 'vendedor@test.com', senha: '123', cidade: 'Santa Rosa', tipo: 'vendedor-comprador' },
    'comprador@test.com': { id: 'c1', nome: 'Maria Santos', email: 'comprador@test.com', senha: '123', cidade: 'Santa Rosa', tipo: 'vendedor-comprador' }
  });

  const [produtos, setProdutos] = useState(JSON.parse(localStorage.getItem('produtos')) || [
    // ===== PRODUTOS DE TESTE =====
    {
      id: 1001,
      nome: '🍽️ Marmita Teste',
      preco_anuncio: 45,
      preco_minimo: 35,
      preco_repassado: 35,
      emoji: '🍽️',
      categoria: 'Viandas',
      estado: 'RS',
      fotos: [],
      faz_entrega: true,
      valor_frete: 0,
      preco_minimo_frete: 0,
      raio_entrega: 5,
      cidade_vendedor: 'Santa Rosa',
      vendedorId: 'teste123',
      vendedorNome: '🧪 Teste Completo',
      ip_vendedor: '192.168.1.1',
      com_selo_confianca: true,
      com_desconto_progressivo: false,
      permitir_combo: false,
      descricao_combo: '',
      desconto_combo: 0,
      aceita_reducao_automatica: true,
      data_publicacao: new Date().getTime(),
      dias_reducao: 0,
      eh_vianda: true,
      dias_semana: ['Seg', 'Ter', 'Qua', 'Qui', 'Sex'],
      quantidade_diaria: 15,
      proxima_renovacao: new Date(new Date().getTime() + 30 * 24 * 60 * 60 * 1000).getTime(),
      dataCriacao: new Date().getTime(),
      visualizacoes: 23,
      em_negociacao: 1,
      status: 'ativo'
    },
    {
      id: 1002,
      nome: '📱 Smartphone Teste',
      preco_anuncio: 1200,
      preco_minimo: 800,
      preco_repassado: 800,
      emoji: '📱',
      categoria: 'Eletrônicos',
      estado: 'RS',
      fotos: [],
      faz_entrega: true,
      valor_frete: 50,
      preco_minimo_frete: 15,
      raio_entrega: 20,
      cidade_vendedor: 'Santa Rosa',
      vendedorId: 'teste123',
      vendedorNome: '🧪 Teste Completo',
      ip_vendedor: '192.168.1.1',
      com_selo_confianca: false,
      com_desconto_progressivo: true,
      permitir_combo: true,
      descricao_combo: '2 unidades = 10% desconto',
      desconto_combo: 120,
      aceita_reducao_automatica: false,
      data_publicacao: new Date().getTime(),
      dias_reducao: 0,
      eh_vianda: false,
      dias_semana: [],
      quantidade_diaria: 0,
      proxima_renovacao: new Date(new Date().getTime() + 30 * 24 * 60 * 60 * 1000).getTime(),
      dataCriacao: new Date().getTime(),
      visualizacoes: 156,
      em_negociacao: 5,
      status: 'ativo'
    },
    {
      id: 1003,
      nome: '👕 Camiseta Teste',
      preco_anuncio: 89,
      preco_minimo: 45,
      preco_repassado: 45,
      emoji: '👕',
      categoria: 'Moda',
      estado: 'RS',
      fotos: [],
      faz_entrega: true,
      valor_frete: 0,
      preco_minimo_frete: 0,
      raio_entrega: 100,
      cidade_vendedor: 'Santa Rosa',
      vendedorId: 'teste123',
      vendedorNome: '🧪 Teste Completo',
      ip_vendedor: '192.168.1.1',
      com_selo_confianca: true,
      com_desconto_progressivo: false,
      permitir_combo: false,
      descricao_combo: '',
      desconto_combo: 0,
      aceita_reducao_automatica: true,
      data_publicacao: new Date(new Date().getTime() - 10 * 24 * 60 * 60 * 1000).getTime(),
      dias_reducao: 0,
      eh_vianda: false,
      dias_semana: [],
      quantidade_diaria: 0,
      proxima_renovacao: new Date(new Date().getTime() + 20 * 24 * 60 * 60 * 1000).getTime(),
      dataCriacao: new Date(new Date().getTime() - 10 * 24 * 60 * 60 * 1000).getTime(),
      visualizacoes: 89,
      em_negociacao: 3,
      status: 'ativo'
    },
    {
      id: 1004,
      nome: '🏠 Luminária Teste',
      preco_anuncio: 150,
      preco_minimo: 100,
      preco_repassado: 100,
      emoji: '💡',
      categoria: 'Casa',
      estado: 'RS',
      fotos: [],
      faz_entrega: true,
      valor_frete: 25,
      preco_minimo_frete: 30,
      raio_entrega: 50,
      cidade_vendedor: 'Santa Rosa',
      vendedorId: 'teste123',
      vendedorNome: '🧪 Teste Completo',
      ip_vendedor: '192.168.1.1',
      com_selo_confianca: false,
      com_desconto_progressivo: true,
      permitir_combo: false,
      descricao_combo: '',
      desconto_combo: 0,
      aceita_reducao_automatica: false,
      data_publicacao: new Date().getTime(),
      dias_reducao: 0,
      eh_vianda: false,
      dias_semana: [],
      quantidade_diaria: 0,
      proxima_renovacao: new Date(new Date().getTime() + 30 * 24 * 60 * 60 * 1000).getTime(),
      dataCriacao: new Date().getTime(),
      visualizacoes: 34,
      em_negociacao: 0,
      status: 'ativo'
    },
    {
      id: 1005,
      nome: '⚽ Bola Teste',
      preco_anuncio: 120,
      preco_minimo: 80,
      preco_repassado: 80,
      emoji: '⚽',
      categoria: 'Esportes',
      estado: 'RS',
      fotos: [],
      faz_entrega: true,
      valor_frete: 0,
      preco_minimo_frete: 0,
      raio_entrega: 10,
      cidade_vendedor: 'Santa Rosa',
      vendedorId: 'teste123',
      vendedorNome: '🧪 Teste Completo',
      ip_vendedor: '192.168.1.1',
      com_selo_confianca: true,
      com_desconto_progressivo: false,
      permitir_combo: true,
      descricao_combo: '3 bolas = 15% desconto',
      desconto_combo: 54,
      aceita_reducao_automatica: true,
      data_publicacao: new Date(new Date().getTime() - 5 * 24 * 60 * 60 * 1000).getTime(),
      dias_reducao: 0,
      eh_vianda: false,
      dias_semana: [],
      quantidade_diaria: 0,
      proxima_renovacao: new Date(new Date().getTime() + 25 * 24 * 60 * 60 * 1000).getTime(),
      dataCriacao: new Date(new Date().getTime() - 5 * 24 * 60 * 60 * 1000).getTime(),
      visualizacoes: 67,
      em_negociacao: 2,
      status: 'ativo'
    }
  ]);

  const [loginEmail, setLoginEmail] = useState('');
  const [loginSenha, setLoginSenha] = useState('');
  const [cadastroNome, setCadastroNome] = useState('');
  const [cadastroEmail, setCadastroEmail] = useState('');
  const [cadastroSenha, setCadastroSenha] = useState('');
  const [cadastroCidade, setCadastroCidade] = useState('');

  const [novoProduto, setNovoProduto] = useState({ 
    nome: '', 
    preco_anuncio: '', 
    preco_minimo: '', 
    emoji: '📦',
    fotos: [null, null, null, null],
    faz_entrega: false,
    valor_frete: '0',
    raio_entrega: '10',
    preco_minimo_frete: '',
    categoria: 'Eletrônicos',
    estado: 'RS',
    // ✅ NOVO - Aceita redução automática?
    aceita_reducao_automatica: false,
    // ✅ NOVO - Campos para Viandas/Alimentação
    dias_semana: [], // Array com dias que repete (seg, ter, qua, etc)
    quantidade_diaria: '', // Quantidade disponível por dia
    eh_vianda: false // Flag para saber se é vianda
  });
  
  const [chatAberto, setChatAberto] = useState(null);
  const [mensagens, setMensagens] = useState([]);
  const [inputChat, setInputChat] = useState('');
  const [ofertaAtual, setOfertaAtual] = useState('');

  const [notificacoes, setNotificacoes] = useState([]);
  const [meusProdutos, setMeusProdutos] = useState([]);
  const [negociacoesAtivas, setNegociacoesAtivas] = useState({});
  const [filtroCategoria, setFiltroCategoria] = useState('Todos');
  const [filtroCidade, setFiltroCidade] = useState('RS');
  const [busca, setBusca] = useState('');
  const [filtroTempo, setFiltroTempo] = useState('todos');
  const [produtoPublicado, setProdutoPublicado] = useState(false);
  const [produtoEditando, setProdutoEditando] = useState(null);

  const fileInputRefs = [useRef(null), useRef(null), useRef(null), useRef(null)];

  // Salvar estado
  useEffect(() => {
    localStorage.setItem('usuarioLogado', JSON.stringify(usuarioLogado));
  }, [usuarioLogado]);

  // CAPTURAR IP DO USUÁRIO
  useEffect(() => {
    const capturarIP = async () => {
      try {
        const response = await fetch('https://api.ipify.org?format=json');
        const data = await response.json();
        setUsuarioIP(data.ip);
        
        // Salvar IP no usuário logado
        if (usuarioLogado) {
          const usuarioComIP = { ...usuarioLogado, ip: data.ip };
          setUsuarioLogado(usuarioComIP);
        }
      } catch (error) {
        console.log('Não foi possível capturar IP (modo offline)');
      }
    };
    
    capturarIP();
  }, []);

  useEffect(() => {
    localStorage.setItem('usuarios', JSON.stringify(usuarios));
  }, [usuarios]);

  useEffect(() => {
    localStorage.setItem('produtos', JSON.stringify(produtos));
  }, [produtos]);

  useEffect(() => {
    if (usuarioLogado) {
      const prods = produtos.filter(p => p.vendedorId === usuarioLogado.id);
      setMeusProdutos(prods);
    }
  }, [produtos, usuarioLogado]);

  // LOGIN
  const fazerLogin = () => {
    const user = usuarios[loginEmail];
    if (user && user.senha === loginSenha) {
      setUsuarioLogado(user);
      setTela('inicio');
      setLoginEmail('');
      setLoginSenha('');
    } else {
      alert('Email ou senha incorretos');
    }
  };

  // CADASTRO
  const fazerCadastro = () => {
    if (!cadastroNome || !cadastroEmail || !cadastroSenha || !cadastroCidade) {
      alert('Preencha todos os campos');
      return;
    }
    if (usuarios[cadastroEmail]) {
      alert('Email já cadastrado');
      return;
    }

    const novoUsuario = {
      id: Date.now().toString(),
      nome: cadastroNome,
      email: cadastroEmail,
      senha: cadastroSenha,
      cidade: cadastroCidade,
      tipo: 'vendedor-comprador'
    };

    const novosUsuarios = { ...usuarios, [cadastroEmail]: novoUsuario };
    setUsuarios(novosUsuarios);
    setUsuarioLogado(novoUsuario);
    setTela('inicio');
    setCadastroNome('');
    setCadastroEmail('');
    setCadastroSenha('');
    setCadastroCidade('');
  };

  // VERIFICAR SE PODE NEGOCIAR (BLOQUEIO POR IP + ID)
  const podeNegociar = (produto) => {
    if (!usuarioLogado) return true; // Não logado, mostra erro de login
    
    // ❌ NÃO PODE: Mesmo ID (mesma conta/pessoa)
    if (usuarioLogado.id === produto.vendedorId) {
      return false; // BLOQUEADO - Mesmo usuário
    }
    
    // ❌ NÃO PODE: Mesmo IP (mesma rede WiFi ou dados móveis)
    if (usuarioIP && produto.ip_vendedor && usuarioIP === produto.ip_vendedor) {
      return false; // BLOQUEADO - Mesma rede
    }
    
    // ✅ PODE: ID diferente E IP diferente
    return true;
  };

  // CALCULAR PREÇO COM REDUÇÃO AUTOMÁTICA
  const calcularPrecoComReducao = (produto) => {
    if (!produto.aceita_reducao_automatica) {
      return {
        preco_anuncio_atual: produto.preco_anuncio,
        preco_minimo_atual: produto.preco_minimo,
        dias_reducao: 0
      };
    }

    // Usar dataCriacao se data_publicacao não existir
    const dataPublicacao = produto.data_publicacao || produto.dataCriacao;
    
    if (!dataPublicacao) {
      // Se não tiver data, retorna preço original
      return {
        preco_anuncio_atual: produto.preco_anuncio,
        preco_minimo_atual: produto.preco_minimo,
        dias_reducao: 0
      };
    }

    const agora = new Date().getTime();
    const publicacao = typeof dataPublicacao === 'string' ? new Date(dataPublicacao).getTime() : dataPublicacao;
    const diasPublicado = Math.floor((agora - publicacao) / (1000 * 60 * 60 * 24));
    
    // Reduz 1% ao dia
    const percentualReducao = diasPublicado * 1;
    const preco_anuncio_atual = Math.floor(produto.preco_anuncio * (1 - percentualReducao / 100));
    const preco_minimo_atual = Math.floor(produto.preco_minimo * (1 - percentualReducao / 100));

    return {
      preco_anuncio_atual: Math.max(preco_anuncio_atual, 0),
      preco_minimo_atual: Math.max(preco_minimo_atual, 0),
      dias_reducao: diasPublicado
    };
  };

  // CATEGORIZAR PRODUTO POR TEMPO
  const categorizarPorTempo = (dataCriacao) => {
    const agora = new Date().getTime();
    const diff = agora - dataCriacao;
    const horas = Math.floor(diff / (1000 * 60 * 60));
    const dias = Math.floor(horas / 24);
    
    if (dias === 0) return 'novo'; // 0-24h
    if (dias === 1) return 'rapido'; // 24-48h
    if (dias >= 2 && dias <= 25) return 'vendendo'; // 3-25 dias
    if (dias >= 26 && dias <= 29) return 'liquidando'; // 26-29 dias
    if (dias >= 30) return 'saldao'; // 30+ dias
  };

  // FORMATRAR DATA DE RENOVAÇÃO
  const formatarRenovacao = (timestamp) => {
    const agora = new Date();
    const renovacao = new Date(timestamp);
    const diff = renovacao - agora;
    const dias = Math.floor(diff / (1000 * 60 * 60 * 24));
    const horas = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    
    if (dias > 0) {
      return `Renova em ${dias}d ${horas}h`;
    } else if (horas > 0) {
      return `Renova em ${horas}h`;
    } else {
      return 'Renovando agora!';
    }
  };

  // MOTIVO DO BLOQUEIO
  const motivoBloqueio = (produto) => {
    if (usuarioLogado.id === produto.vendedorId) {
      return "Este é seu produto! Você não pode negociar consigo mesmo.";
    }
    if (usuarioIP && produto.ip_vendedor && usuarioIP === produto.ip_vendedor) {
      return `🔒 Bloqueado por segurança: Detectamos que você está na mesma rede (IP: ${usuarioIP}) do vendedor. Pessoas da mesma casa/WiFi/rede móvel não podem negociar para evitar fraudes.`;
    }
    return "";
  };

  // UPLOAD DE FOTOS
  const handleFotoUpload = (index, e) => {
    const file = e.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = (event) => {
        const novasFotos = [...novoProduto.fotos];
        novasFotos[index] = event.target.result;
        setNovoProduto({ ...novoProduto, fotos: novasFotos });
      };
      reader.readAsDataURL(file);
    }
  };

  // PRODUTOS
  const adicionarProduto = () => {
    if (!novoProduto.nome || !novoProduto.preco_anuncio || !novoProduto.preco_minimo) {
      alert('Preencha nome e preços');
      return;
    }

    const preco_anuncio = parseInt(novoProduto.preco_anuncio);
    const preco_minimo = parseInt(novoProduto.preco_minimo);

    // VALIDAÇÃO: Mínimo nunca pode ser maior que anúncio
    if (preco_minimo > preco_anuncio) {
      alert('❌ ERRO: O preço mínimo não pode ser maior que o preço de anúncio!\n\nExemplo correto:\nAnúncio: R$ 5.000\nMínimo: R$ 2.000');
      return;
    }

    // VALIDAÇÃO: Mínimo não pode ser zero ou negativo
    if (preco_minimo <= 0) {
      alert('❌ O preço mínimo deve ser maior que R$ 0');
      return;
    }

    // VALIDAÇÃO: Anúncio não pode ser zero ou negativo
    if (preco_anuncio <= 0) {
      alert('❌ O preço de anúncio deve ser maior que R$ 0');
      return;
    }

    const lucro_inicial = preco_anuncio - preco_minimo;

    const novoProd = {
      id: Date.now(),
      nome: novoProduto.nome,
      preco_anuncio,
      preco_minimo,
      preco_repassado: preco_minimo,
      emoji: novoProduto.emoji,
      categoria: novoProduto.categoria,
      estado: novoProduto.estado,
      fotos: novoProduto.fotos.filter(f => f !== null),
      faz_entrega: novoProduto.faz_entrega,
      valor_frete: novoProduto.faz_entrega && novoProduto.valor_frete !== '0' ? parseInt(novoProduto.valor_frete) : 0,
      preco_minimo_frete: novoProduto.faz_entrega && novoProduto.valor_frete !== '0' ? parseInt(novoProduto.preco_minimo_frete) : 0,
      raio_entrega: novoProduto.faz_entrega ? parseInt(novoProduto.raio_entrega) : 0,
      cidade_vendedor: usuarioLogado.cidade,
      vendedorId: usuarioLogado.id,
      vendedorNome: usuarioLogado.nome,
      ip_vendedor: usuarioIP,
      // ✅ NOVOS CAMPOS DE SUGESTÕES
      com_selo_confianca: novoProduto.com_selo_confianca || false,
      com_desconto_progressivo: novoProduto.com_desconto_progressivo || false,
      permitir_combo: novoProduto.permitir_combo || false,
      descricao_combo: novoProduto.descricao_combo || '',
      desconto_combo: novoProduto.desconto_combo ? parseInt(novoProduto.desconto_combo) : 0,
      // ✅ REDUÇÃO AUTOMÁTICA
      aceita_reducao_automatica: novoProduto.aceita_reducao_automatica || false,
      data_publicacao: new Date().getTime(),
      dias_reducao: 0,
      // ✅ VIANDAS/ALIMENTAÇÃO
      eh_vianda: novoProduto.eh_vianda || false,
      dias_semana: novoProduto.dias_semana || [],
      quantidade_diaria: novoProduto.quantidade_diaria ? parseInt(novoProduto.quantidade_diaria) : 0,
      // ============
      proxima_renovacao: new Date(new Date().getTime() + 30 * 24 * 60 * 60 * 1000).getTime(), // 30 dias
      dataCriacao: new Date().getTime(),
      visualizacoes: 0,
      em_negociacao: 0,
      status: 'ativo'
    };

    setProdutos([novoProd, ...produtos]);
    setNovoProduto({ 
      nome: '', preco_anuncio: '', preco_minimo: '', emoji: '📦', 
      fotos: [null, null, null, null], faz_entrega: false, valor_frete: '0', 
      raio_entrega: '10', preco_minimo_frete: '', categoria: 'Eletrônicos', estado: 'RS',
      aceita_reducao_automatica: false,
      com_selo_confianca: false,
      com_desconto_progressivo: false,
      permitir_combo: false,
      descricao_combo: '',
      desconto_combo: '',
      eh_vianda: false,
      dias_semana: [],
      quantidade_diaria: ''
    });
    setProdutoPublicado(true);
  };

  // REDUZIR PREÇO
  const reduzirPreco = (produtoId) => {
    setProdutos(prev => prev.map(p => {
      if (p.id === produtoId) {
        // Reduz preço de anúncio em 10%
        const novoAnuncio = Math.floor(p.preco_anuncio * 0.9);
        
        // Calcula proporção: (mínimo / anúncio original)
        const percentualMinimoOriginal = p.preco_minimo / p.preco_anuncio;
        
        // Aplica mesma proporção ao novo anúncio
        const novoMinimo = Math.floor(novoAnuncio * percentualMinimoOriginal);

        return {
          ...p,
          preco_anuncio: novoAnuncio,
          preco_minimo: novoMinimo,
          preco_repassado: novoMinimo,
          dataCriacao: new Date().getTime() // Volta ao topo
        };
      }
      return p;
    }));
  };

  // EDITAR PRODUTO
  const abrirEdicao = (produto) => {
    setProdutoEditando({
      ...produto,
      raio_entrega: produto.raio_entrega?.toString() || '10',
      valor_frete: produto.valor_frete?.toString() || '0',
      preco_minimo_frete: produto.preco_minimo_frete?.toString() || ''
    });
    setNovoProduto({
      nome: produto.nome,
      preco_anuncio: produto.preco_anuncio.toString(),
      preco_minimo: produto.preco_minimo.toString(),
      emoji: produto.emoji,
      fotos: produto.fotos || [null, null, null, null],
      faz_entrega: produto.faz_entrega,
      valor_frete: produto.valor_frete?.toString() || '0',
      raio_entrega: produto.raio_entrega?.toString() || '10',
      preco_minimo_frete: produto.preco_minimo_frete?.toString() || '',
      categoria: produto.categoria,
      estado: produto.estado,
      // ✅ MANTER TODAS AS OPÇÕES
      com_selo_confianca: produto.com_selo_confianca || false,
      com_desconto_progressivo: produto.com_desconto_progressivo || false,
      permitir_combo: produto.permitir_combo || false,
      descricao_combo: produto.descricao_combo || '',
      desconto_combo: produto.desconto_combo?.toString() || '',
      aceita_reducao_automatica: produto.aceita_reducao_automatica || false,
      // ✅ CAMPOS VIANDA
      eh_vianda: produto.eh_vianda || false,
      dias_semana: produto.dias_semana || [],
      quantidade_diaria: produto.quantidade_diaria?.toString() || ''
    });
  };

  const salvarEdicao = () => {
    if (!novoProduto.nome || !novoProduto.preco_anuncio || !novoProduto.preco_minimo) {
      alert('Preencha nome e preços');
      return;
    }

    const preco_anuncio = parseInt(novoProduto.preco_anuncio);
    const preco_minimo = parseInt(novoProduto.preco_minimo);

    if (preco_minimo > preco_anuncio) {
      alert('Preço mínimo não pode ser maior que o anúncio');
      return;
    }

    setProdutos(prev => prev.map(p => {
      if (p.id === produtoEditando.id) {
        return {
          ...p,
          nome: novoProduto.nome,
          preco_anuncio,
          preco_minimo,
          preco_repassado: preco_minimo,
          emoji: novoProduto.emoji,
          categoria: novoProduto.categoria,
          estado: novoProduto.estado,
          fotos: novoProduto.fotos.filter(f => f !== null),
          faz_entrega: novoProduto.faz_entrega,
          valor_frete: novoProduto.faz_entrega ? parseInt(novoProduto.valor_frete) : 0,
          preco_minimo_frete: novoProduto.faz_entrega && novoProduto.valor_frete !== '0' ? parseInt(novoProduto.preco_minimo_frete) : 0,
          raio_entrega: novoProduto.faz_entrega ? parseInt(novoProduto.raio_entrega) : 0,
          // ✅ MANTER TODAS AS OPÇÕES
          com_selo_confianca: novoProduto.com_selo_confianca !== undefined ? novoProduto.com_selo_confianca : p.com_selo_confianca,
          com_desconto_progressivo: novoProduto.com_desconto_progressivo !== undefined ? novoProduto.com_desconto_progressivo : p.com_desconto_progressivo,
          permitir_combo: novoProduto.permitir_combo !== undefined ? novoProduto.permitir_combo : p.permitir_combo,
          descricao_combo: novoProduto.descricao_combo || p.descricao_combo,
          desconto_combo: novoProduto.desconto_combo ? parseInt(novoProduto.desconto_combo) : p.desconto_combo,
          aceita_reducao_automatica: novoProduto.aceita_reducao_automatica !== undefined ? novoProduto.aceita_reducao_automatica : p.aceita_reducao_automatica,
          // ✅ VIANDA
          eh_vianda: novoProduto.eh_vianda !== undefined ? novoProduto.eh_vianda : p.eh_vianda,
          dias_semana: novoProduto.dias_semana || p.dias_semana,
          quantidade_diaria: novoProduto.quantidade_diaria ? parseInt(novoProduto.quantidade_diaria) : p.quantidade_diaria
        };
      }
      return p;
    }));

    setProdutoEditando(null);
    setNovoProduto({ 
      nome: '', 
      preco_anuncio: '', 
      preco_minimo: '', 
      emoji: '📦', 
      fotos: [null, null, null, null], 
      faz_entrega: false, 
      valor_frete: '0', 
      raio_entrega: '10', 
      preco_minimo_frete: '', 
      categoria: 'Eletrônicos', 
      estado: 'RS',
      com_selo_confianca: false,
      com_desconto_progressivo: false,
      permitir_combo: false,
      descricao_combo: '',
      desconto_combo: '',
      aceita_reducao_automatica: false
    });
    alert('Produto atualizado com sucesso!');
    setTela('meus-produtos');
  };

  const cancelarEdicao = () => {
    setProdutoEditando(null);
    setNovoProduto({ 
      nome: '', 
      preco_anuncio: '', 
      preco_minimo: '', 
      emoji: '📦', 
      fotos: [null, null, null, null], 
      faz_entrega: false, 
      valor_frete: '0', 
      raio_entrega: '10', 
      preco_minimo_frete: '', 
      categoria: 'Eletrônicos', 
      estado: 'RS',
      com_selo_confianca: false,
      com_desconto_progressivo: false,
      permitir_combo: false,
      descricao_combo: '',
      desconto_combo: '',
      aceita_reducao_automatica: false,
      eh_vianda: false,
      dias_semana: [],
      quantidade_diaria: ''
    });
    setTela('meus-produtos');
  };

  // CHAT
  // ABRIR CHAT
  const abrirChat = (produto) => {
    if (!usuarioLogado) {
      alert('Faça login para negociar!');
      setTela('login');
      return;
    }

    // ✅ VERIFICAR BLOQUEIO
    if (!podeNegociar(produto)) {
      alert(`❌ ${motivoBloqueio(produto)}`);
      return;
    }

    // ✅ PODE NEGOCIAR - ABRE CHAT
    setChatAberto(produto);
    setNegociacoesAtivas(prev => ({
      ...prev,
      [produto.id]: true
    }));
    setMensagens([
      {
        id: 1,
        tipo: 'bot',
        texto: `👋 Olá! Bem-vindo à negociação!

📦 Produto: ${produto.nome}
💚 Preço: R$ ${produto.preco_anuncio.toLocaleString('pt-BR')}
✅ Frete: GRÁTIS

💰 Qual é sua melhor oferta?
(Digite um número, ex: 2500)`
      }
    ]);
    setOfertaAtual(produto.preco_anuncio.toString());
  };

  const fecharChat = () => {
    setNegociacoesAtivas(prev => {
      const novo = { ...prev };
      delete novo[chatAberto.id];
      return novo;
    });
    setChatAberto(null);
    setMensagens([]);
    setInputChat('');
  };

  const logout = () => {
    setUsuarioLogado(null);
    setTela('inicio');
  };

  const gerarRespostaNegociacao = (mensagem, produto) => {
    const msgLower = mensagem.toLowerCase();
    const oferta = parseInt(ofertaAtual) || produto.preco_anuncio;

    let resposta = '';

    // PALAVRAS-CHAVE PARA FECHAR VENDA
    if (msgLower.includes('compro') || msgLower.includes('topei') || msgLower.includes('fechado') || msgLower.includes('ok') || msgLower.includes('topo')) {
      resposta = `🎉 EXCELENTE!

Vamos fechar em R$ ${oferta.toLocaleString('pt-BR')} 
✅ Com FRETE GRÁTIS!

Agora vamos para a parte segura! 🔒

Qual é seu método de pagamento?

☑️ PIX
☐ Cartão de Crédito
☐ Boleto`;
      return resposta;
    }

    // SE FOR UM NÚMERO = OFERTA
    if (!isNaN(inputChat)) {
      const novaOferta = parseInt(inputChat);
      
      // MUITO ABAIXO DO MÍNIMO
      if (novaOferta < produto.preco_minimo - 200) {
        resposta = `⏳ Um momento...
🤔 Estou analisando sua proposta...
📊 Calculando aqui...

Infelizmente R$ ${novaOferta.toLocaleString('pt-BR')} está bem abaixo.
Este é um ${produto.nome}, produto de qualidade!

💡 Você não consegue melhorar um pouco?
Qual seria sua próxima oferta?`;
      }
      
      // ABAIXO DO MÍNIMO MAS PERTO
      else if (novaOferta < produto.preco_minimo) {
        resposta = `⏳ Analisando...
🤔 O vendedor está pensando...
📊 Fazendo os cálculos...

Ainda está um pouco longe do mínimo.

💡 Qual seria sua melhor oferta?
Faça uma proposta mais competitiva!`;
      }
      
      // NO MÍNIMO EXATO
      else if (novaOferta === produto.preco_minimo) {
        resposta = `⏳ Entendo sua posição...
🤔 O vendedor também quer fazer negócio...

Deixa eu ver se consigo algo com ele...
📊 Analisando...

Tá bem! Vamos fechar em R$ ${produto.preco_minimo.toLocaleString('pt-BR')}?
✅ Com FRETE GRÁTIS!`;
      }
      
      // POUCO ACIMA DO MÍNIMO
      else if (novaOferta < produto.preco_anuncio * 0.7) {
        resposta = `⏳ Deixa eu calcular...
🤔 Analisando com o vendedor...
📊 Calculando viabilidade...

Ótima oferta! Mas o vendedor gostaria um pouco mais.

💡 Consegue incrementar mais um pouco?
Qual seria sua melhor oferta realmente?`;
      }
      
      // PERTO DO ANÚNCIO
      else if (novaOferta >= produto.preco_anuncio * 0.8) {
        resposta = `⏳ Analisando...
🤔 O vendedor ficou feliz!
✅ Excelente proposta!

Vamos confirmar em R$ ${novaOferta.toLocaleString('pt-BR')}?
✅ Com FRETE GRÁTIS!`;
      }
      
      // ACIMA DO ANÚNCIO
      else if (novaOferta >= produto.preco_anuncio) {
        resposta = `🎉 WOW! Que proposta incrível!
✅ O vendedor topou na hora!

Vamos fechar em R$ ${novaOferta.toLocaleString('pt-BR')}?
✅ Com FRETE GRÁTIS!
Produto saindo hoje!`;
      }
      
      // NO MEIO DO CAMINHO
      else {
        resposta = `⏳ Um momento...
🤔 O vendedor está analisando...
📊 Calculando a viabilidade...

Boa oferta! Mas o vendedor espera um pouco mais.

💡 Você não consegue melhorar?
Qual seria sua próxima oferta?`;
      }
    } else {
      // SE FOR TEXTO
      resposta = `💰 Qual é sua proposta em reais?
Digite um número, tipo: 1500`;
    }

    return resposta;
  };

  const enviarMensagem = () => {
    if (!inputChat.trim()) return;

    setMensagens(prev => [...prev, {
      id: Date.now(),
      tipo: 'comprador',
      texto: inputChat
    }]);

    // Capturar valor numérico se for oferta
    if (!isNaN(inputChat) && inputChat.trim() !== '') {
      setOfertaAtual(inputChat);
    }

    setTimeout(() => {
      const resposta = gerarRespostaNegociacao(inputChat, chatAberto);
      setMensagens(prev => [...prev, {
        id: Date.now() + 1,
        tipo: 'vendedor',
        texto: resposta
      }]);

      // Se fechou a venda, vai para pagamento
      if (inputChat.toLowerCase().includes('compro') || inputChat.toLowerCase().includes('topei') || inputChat.toLowerCase().includes('fechado')) {
        setTimeout(() => {
          setMensagens(prev => [...prev, {
            id: Date.now() + 2,
            tipo: 'sistema',
            texto: '⏳ Redirecionando para pagamento seguro...'
          }]);
        }, 1500);
      }
    }, 800);

    setInputChat('');
  };

  // ============ TELAS ============

  const navbar = (
    <nav className="bg-gradient-to-r from-slate-900 via-purple-900 to-slate-900 text-white shadow-2xl p-5 flex justify-between items-center border-b-2 border-purple-500/50 backdrop-blur-lg">
      <div className="flex items-center gap-3">
        <div className="text-3xl font-black bg-gradient-to-r from-cyan-400 to-purple-500 bg-clip-text text-transparent drop-shadow-lg">
          💎 MarketHub
        </div>
        <div className="hidden sm:block text-xs font-semibold text-purple-300 uppercase tracking-widest">
          Premium Marketplace
        </div>
      </div>
      
      {usuarioLogado ? (
        <div className="flex items-center gap-6">
          <div className="hidden sm:block text-right">
            <p className="font-bold text-base text-white">{usuarioLogado.nome}</p>
            <p className="text-xs text-purple-300">📍 {usuarioLogado.cidade}</p>
          </div>
          <button
            onClick={logout}
            className="flex items-center gap-2 bg-gradient-to-r from-red-600 to-red-700 hover:from-red-500 hover:to-red-600 px-6 py-2.5 rounded-xl font-bold text-sm transition-all duration-300 shadow-lg hover:shadow-red-500/50 border border-red-400/30"
          >
            <LogOut size={18} /> Sair
          </button>
        </div>
      ) : (
        <button
          onClick={() => setTela('login')}
          className="flex items-center gap-2 bg-gradient-to-r from-cyan-500 to-blue-600 hover:from-cyan-400 hover:to-blue-500 px-6 py-2.5 rounded-xl font-bold text-sm transition-all duration-300 shadow-lg hover:shadow-cyan-500/50 border border-cyan-300/30"
        >
          <LogIn size={18} /> Entrar
        </button>
      )}
    </nav>
  );

  // LOGIN
  if (tela === 'login') {
    return (
      <div className="min-h-screen bg-gradient-to-br from-slate-950 via-purple-950 to-slate-950 flex flex-col">
        {navbar}
        <div className="flex-1 flex items-center justify-center p-4">
          <div className="w-full max-w-md">
            <div className="bg-slate-900/80 backdrop-blur-xl rounded-2xl shadow-2xl p-8 border border-purple-500/20">
              <div className="text-center mb-8">
                <h1 className="text-4xl font-black mb-2 bg-gradient-to-r from-cyan-400 to-purple-500 bg-clip-text text-transparent">
                  Bem-vindo
                </h1>
                <p className="text-purple-300 font-medium">Faça login para continuar</p>
              </div>

              <input
                type="email"
                placeholder="📧 Email"
                value={loginEmail}
                onChange={(e) => setLoginEmail(e.target.value)}
                className="w-full px-5 py-3 bg-slate-800/50 border-2 border-purple-500/30 rounded-xl mb-4 text-white placeholder-purple-400/60 focus:outline-none focus:border-cyan-400 focus:shadow-lg focus:shadow-cyan-500/20 transition-all font-medium"
              />
              <input
                type="password"
                placeholder="🔐 Senha"
                value={loginSenha}
                onChange={(e) => setLoginSenha(e.target.value)}
                onKeyPress={(e) => e.key === 'Enter' && fazerLogin()}
                className="w-full px-5 py-3 bg-slate-800/50 border-2 border-purple-500/30 rounded-xl mb-6 text-white placeholder-purple-400/60 focus:outline-none focus:border-cyan-400 focus:shadow-lg focus:shadow-cyan-500/20 transition-all font-medium"
              />

              <button
                onClick={fazerLogin}
                className="w-full bg-gradient-to-r from-cyan-500 to-blue-600 hover:from-cyan-400 hover:to-blue-500 text-white font-bold py-3 rounded-xl mb-3 transition-all duration-300 shadow-lg hover:shadow-cyan-500/50 border border-cyan-300/30 uppercase tracking-wider text-sm"
              >
                ⚡ Entrar
              </button>

              <button
                onClick={() => setTela('cadastro')}
                className="w-full bg-gradient-to-r from-purple-600 to-pink-600 hover:from-purple-500 hover:to-pink-500 text-white font-bold py-3 rounded-xl transition-all duration-300 shadow-lg hover:shadow-purple-500/50 border border-purple-300/30 uppercase tracking-wider text-sm"
              >
                ✨ Criar Conta
              </button>

              <div className="mt-8 pt-6 border-t border-purple-500/20 text-center">
                <p className="font-bold text-purple-300 mb-3 text-sm">🧪 Contas de Teste:</p>
                <div className="space-y-2 text-xs text-purple-400 bg-slate-800/50 p-3 rounded-xl border border-purple-500/10">
                  <p><span className="text-cyan-400 font-bold">vendedor@test.com</span> / 123</p>
                  <p><span className="text-cyan-400 font-bold">comprador@test.com</span> / 123</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    );
  }

  // CADASTRO
  if (tela === 'cadastro') {
    return (
      <div className="min-h-screen bg-gradient-to-br from-slate-950 via-purple-950 to-slate-950 flex flex-col">
        {navbar}
        <div className="flex-1 flex items-center justify-center p-4">
          <div className="w-full max-w-md">
            <div className="bg-slate-900/80 backdrop-blur-xl rounded-2xl shadow-2xl p-8 border border-purple-500/20">
              <div className="text-center mb-8">
                <h1 className="text-4xl font-black mb-2 bg-gradient-to-r from-purple-400 to-pink-500 bg-clip-text text-transparent">
                  Criar Conta
                </h1>
                <p className="text-purple-300 font-medium">Junte-se ao MarketHub</p>
              </div>

              <input
                type="text"
                placeholder="👤 Nome completo"
                value={cadastroNome}
                onChange={(e) => setCadastroNome(e.target.value)}
                className="w-full px-5 py-3 bg-slate-800/50 border-2 border-purple-500/30 rounded-xl mb-4 text-white placeholder-purple-400/60 focus:outline-none focus:border-pink-400 focus:shadow-lg focus:shadow-pink-500/20 transition-all font-medium"
              />
              <input
                type="email"
                placeholder="📧 Email"
                value={cadastroEmail}
                onChange={(e) => setCadastroEmail(e.target.value)}
                className="w-full px-5 py-3 bg-slate-800/50 border-2 border-purple-500/30 rounded-xl mb-4 text-white placeholder-purple-400/60 focus:outline-none focus:border-pink-400 focus:shadow-lg focus:shadow-pink-500/20 transition-all font-medium"
              />
              <input
                type="text"
                placeholder="📍 Cidade"
                value={cadastroCidade}
                onChange={(e) => setCadastroCidade(e.target.value)}
                className="w-full px-5 py-3 bg-slate-800/50 border-2 border-purple-500/30 rounded-xl mb-4 text-white placeholder-purple-400/60 focus:outline-none focus:border-pink-400 focus:shadow-lg focus:shadow-pink-500/20 transition-all font-medium"
              />
              <input
                type="password"
                placeholder="🔐 Senha"
                value={cadastroSenha}
                onChange={(e) => setCadastroSenha(e.target.value)}
                className="w-full px-5 py-3 bg-slate-800/50 border-2 border-purple-500/30 rounded-xl mb-6 text-white placeholder-purple-400/60 focus:outline-none focus:border-pink-400 focus:shadow-lg focus:shadow-pink-500/20 transition-all font-medium"
              />

              <button
                onClick={fazerCadastro}
                className="w-full bg-gradient-to-r from-purple-600 to-pink-600 hover:from-purple-500 hover:to-pink-500 text-white font-bold py-3 rounded-xl mb-3 transition-all duration-300 shadow-lg hover:shadow-pink-500/50 border border-pink-300/30 uppercase tracking-wider text-sm"
              >
                ✨ Cadastrar
              </button>

              <button
                onClick={() => setTela('inicio')}
                className="w-full bg-slate-800/50 hover:bg-slate-700/50 text-purple-300 font-bold py-3 rounded-xl transition-all duration-300 border border-purple-500/30 hover:border-purple-400/50 uppercase tracking-wider text-sm"
              >
                ← Voltar
              </button>
            </div>
          </div>
        </div>
      </div>
    );
  }

  // NOVO PRODUTO / EDITAR PRODUTO
  if (tela === 'novo-produto' || tela === 'editar-produto') {
    if (!usuarioLogado) {
      return (
        <div className="min-h-screen bg-gray-100">
          {navbar}
          <div className="max-w-2xl mx-auto p-6">
            <div className="bg-white rounded-lg shadow-lg p-8 text-center">
              <p className="text-2xl font-bold text-gray-900 mb-4">Faça login para vender</p>
              <button
                onClick={() => setTela('login')}
                className="bg-green-600 hover:bg-green-700 text-white font-bold px-6 py-3 rounded-lg"
              >
                Fazer Login
              </button>
            </div>
          </div>
        </div>
      );
    }

    // MODAL DE SUCESSO
    if (produtoPublicado && tela === 'novo-produto') {
      return (
        <div className="fixed inset-0 bg-black/50 flex items-center justify-center p-4 z-50">
          <div className="bg-white rounded-lg shadow-2xl p-8 w-full max-w-md text-center">
            <div className="text-6xl mb-4">✅</div>
            <h2 className="text-2xl font-bold text-gray-900 mb-2">Produto Publicado!</h2>
            <p className="text-gray-600 mb-6">Seu produto foi publicado com sucesso e já está aparecendo na vitrine.</p>
            
            <div className="space-y-3">
              <button
                onClick={() => {
                  setProdutoPublicado(false);
                  setTela('novo-produto');
                }}
                className="w-full bg-green-600 hover:bg-green-700 text-white font-bold py-3 rounded-lg transition"
              >
                Publicar outro Produto
              </button>
              <button
                onClick={() => {
                  setProdutoPublicado(false);
                  setTela('meus-produtos');
                }}
                className="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 rounded-lg transition"
              >
                Ver Meus Produtos
              </button>
              <button
                onClick={() => {
                  setProdutoPublicado(false);
                  setTela('inicio');
                }}
                className="w-full bg-gray-400 hover:bg-gray-500 text-white font-bold py-3 rounded-lg transition"
              >
                Voltar à Vitrine
              </button>
            </div>
          </div>
        </div>
      );
    }

    return (
      <div className="min-h-screen bg-gray-100">
        {navbar}
        <div className="max-w-2xl mx-auto p-6">
          <div className="bg-white rounded-lg shadow-lg p-8">
            <h2 className="text-3xl font-bold mb-6">{produtoEditando ? '✏️ Editar Produto' : '📦 Publicar Novo Produto'}</h2>

            <input
              type="text"
              placeholder="Nome do produto"
              value={novoProduto.nome}
              onChange={(e) => setNovoProduto({ ...novoProduto, nome: e.target.value })}
              className="w-full px-4 py-3 border border-gray-300 rounded-lg mb-4 focus:outline-none focus:ring-2 focus:ring-green-500"
            />

            <input
              type="text"
              placeholder="Emoji (ex: 📱)"
              maxLength="2"
              value={novoProduto.emoji}
              onChange={(e) => setNovoProduto({ ...novoProduto, emoji: e.target.value })}
              className="w-full px-4 py-3 border border-gray-300 rounded-lg mb-4 focus:outline-none focus:ring-2 focus:ring-green-500 text-center text-2xl"
            />

            <div className="grid grid-cols-2 gap-4 mb-4">
              <div>
                <label className="block text-sm font-bold text-gray-700 mb-2">Categoria</label>
                <select
                  value={novoProduto.categoria}
                  onChange={(e) => {
                    const novaCategoria = e.target.value;
                    setNovoProduto({ 
                      ...novoProduto, 
                      categoria: novaCategoria,
                      eh_vianda: novaCategoria === 'Viandas' || novaCategoria === 'Alimentação'
                    });
                  }}
                  className="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500"
                >
                  <option value="Eletrônicos">Eletrônicos</option>
                  <option value="Moda">Moda</option>
                  <option value="Casa">Casa</option>
                  <option value="Esportes">Esportes</option>
                  <option value="Livros">Livros</option>
                  <option value="Viandas">🍽️ Viandas</option>
                  <option value="Alimentação">🥗 Alimentação</option>
                  <option value="Outros">Outros</option>
                </select>
              </div>
              <div>
                <label className="block text-sm font-bold text-gray-700 mb-2">Estado</label>
                <select
                  value={novoProduto.estado}
                  onChange={(e) => setNovoProduto({ ...novoProduto, estado: e.target.value })}
                  className="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500"
                >
                  <option value="SP">São Paulo</option>
                  <option value="RJ">Rio de Janeiro</option>
                  <option value="MG">Minas Gerais</option>
                  <option value="BA">Bahia</option>
                  <option value="SC">Santa Catarina</option>
                  <option value="RS">Rio Grande do Sul</option>
                  <option value="PR">Paraná</option>
                  <option value="DF">Distrito Federal</option>
                </select>
              </div>
            </div>

            <div className="grid grid-cols-2 gap-4 mb-6">
              <div>
                <label className="block text-sm font-bold text-gray-700 mb-2">Preço de Anúncio (R$)</label>
                <input
                  type="number"
                  placeholder="5000"
                  value={novoProduto.preco_anuncio}
                  onChange={(e) => setNovoProduto({ ...novoProduto, preco_anuncio: e.target.value })}
                  className="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500"
                />
              </div>
              <div>
                <label className="block text-sm font-bold text-gray-700 mb-2">Preço Mínimo (R$)</label>
                <input
                  type="number"
                  placeholder="2000"
                  value={novoProduto.preco_minimo}
                  onChange={(e) => setNovoProduto({ ...novoProduto, preco_minimo: e.target.value })}
                  className="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500"
                />
              </div>
            </div>

            {/* UPLOAD DE 4 FOTOS */}
            <div className="mb-6">
              <label className="block text-sm font-bold text-gray-700 mb-3">Fotos do Produto (até 4)</label>
              <div className="grid grid-cols-2 gap-4">
                {[0, 1, 2, 3].map((index) => (
                  <div key={index} className="relative">
                    <div
                      onClick={() => fileInputRefs[index].current?.click()}
                      className="w-full h-32 border-2 border-dashed border-gray-300 rounded-lg flex items-center justify-center cursor-pointer hover:border-green-500 hover:bg-green-50 transition"
                    >
                      {novoProduto.fotos[index] ? (
                        <img
                          src={novoProduto.fotos[index]}
                          alt={`Foto ${index + 1}`}
                          className="w-full h-full object-cover rounded-lg"
                        />
                      ) : (
                        <div className="text-center">
                          <p className="text-3xl mb-1">📷</p>
                          <p className="text-xs text-gray-500">Foto {index + 1}</p>
                        </div>
                      )}
                    </div>
                    <input
                      ref={fileInputRefs[index]}
                      type="file"
                      accept="image/*"
                      onChange={(e) => handleFotoUpload(index, e)}
                      className="hidden"
                    />
                  </div>
                ))}
              </div>
            </div>

            {/* SUGESTÃO INTELIGENTE DE FRETE GRÁTIS */}
            {novoProduto.preco_anuncio && novoProduto.preco_minimo && (
              <div className="mb-6 bg-gradient-to-r from-cyan-500/10 to-blue-500/10 border-2 border-cyan-400/50 rounded-xl p-5">
                <div className="flex items-start gap-4">
                  <div className="text-3xl">💡</div>
                  <div className="flex-1">
                    <p className="font-bold text-cyan-700 text-lg mb-2">Sugestão Inteligente!</p>
                    
                    {parseInt(novoProduto.preco_anuncio) >= parseInt(novoProduto.preco_minimo) ? (
                      <>
                        {(() => {
                          const precoAtual = parseInt(novoProduto.preco_anuncio);
                          // Frete inteligente: 10% do preço, máx de 300 reais
                          const freteInteligente = Math.min(Math.floor(precoAtual * 0.1), 300);
                          // Preço sugerido: atual + frete
                          const precoSugerido = precoAtual + freteInteligente;
                          
                          return (
                            <>
                              <p className="text-sm text-cyan-600 mb-3">
                                Aumente seu preço para <strong>R$ {precoSugerido.toLocaleString('pt-BR')}</strong> e ofereça <strong>FRETE GRÁTIS!</strong>
                              </p>
                              <p className="text-xs text-cyan-500 mb-4 bg-white/50 p-2 rounded">
                                ✅ Produtos com frete grátis vendem <strong>40% mais rápido</strong>
                                <br/>
                                ✅ Clientes adoram economizar com frete
                                <br/>
                                ✅ Seu preço mínimo continua em R$ {parseInt(novoProduto.preco_minimo).toLocaleString('pt-BR')}
                                <br/>
                                ✅ Aumento inteligente: +R$ {freteInteligente} (10% do preço)
                              </p>
                              <button
                                onClick={() => {
                                  setNovoProduto({
                                    ...novoProduto,
                                    preco_anuncio: precoSugerido.toString(),
                                    faz_entrega: true,
                                    valor_frete: '0',
                                    raio_entrega: '100'
                                  });
                                }}
                                className="bg-gradient-to-r from-cyan-500 to-blue-600 hover:from-cyan-400 hover:to-blue-500 text-white font-bold px-4 py-2 rounded-lg text-sm transition-all duration-300 shadow-lg"
                              >
                                ✨ Aplicar Sugestão
                              </button>
                            </>
                          );
                        })()}
                      </>
                    ) : (
                      <p className="text-sm text-red-600">
                        ⚠️ Preencha primeiro o preço de anúncio maior que o preço mínimo!
                        <br/>
                        <span className="text-xs">Anúncio: R$ {novoProduto.preco_anuncio} | Mínimo: R$ {novoProduto.preco_minimo}</span>
                      </p>
                    )}
                  </div>
                </div>
              </div>
            )}

            {/* SUGESTÃO 2: MARCA DE CONFIANÇA */}
            {novoProduto.preco_anuncio && novoProduto.preco_minimo && parseInt(novoProduto.preco_anuncio) >= parseInt(novoProduto.preco_minimo) && (
              <div className="mb-6 bg-gradient-to-r from-amber-500/10 to-orange-500/10 border-2 border-amber-400/50 rounded-xl p-5">
                <div className="flex items-start gap-4">
                  <div className="text-3xl">⭐</div>
                  <div className="flex-1">
                    <p className="font-bold text-amber-700 text-lg mb-2">Aumente Sua Confiabilidade!</p>
                    <p className="text-sm text-amber-600 mb-3">
                      Adicione um <strong>Selo de Confiança</strong> ao seu perfil. Compradores confiam muito mais em vendedores com selo!
                    </p>
                    <p className="text-xs text-amber-500 mb-4 bg-white/50 p-2 rounded">
                      ✅ <strong>+35% mais visualizações</strong>
                      <br/>
                      ✅ <strong>+25% mais negociações</strong>
                      <br/>
                      ✅ <strong>Faster shipping = Happy customers</strong>
                      <br/>
                      ✅ <strong>Mostre sua classificação:</strong> 5 estrelas
                    </p>
                    <label className="flex items-center gap-3 cursor-pointer">
                      <input
                        type="checkbox"
                        checked={novoProduto.com_selo_confianca || false}
                        onChange={(e) => setNovoProduto({ ...novoProduto, com_selo_confianca: e.target.checked })}
                        className="w-5 h-5 text-amber-600 rounded"
                      />
                      <span className="text-sm font-bold text-amber-700">
                        ✅ Quero adicionar Selo de Confiança ao meu perfil
                      </span>
                    </label>
                  </div>
                </div>
              </div>
            )}

            {/* SUGESTÃO 3: DESCONTO PROGRESSIVO */}
            {novoProduto.preco_anuncio && novoProduto.preco_minimo && parseInt(novoProduto.preco_anuncio) >= parseInt(novoProduto.preco_minimo) && (
              <div className="mb-6 bg-gradient-to-r from-pink-500/10 to-rose-500/10 border-2 border-pink-400/50 rounded-xl p-5">
                <div className="flex items-start gap-4">
                  <div className="text-3xl">🎯</div>
                  <div className="flex-1">
                    <p className="font-bold text-pink-700 text-lg mb-2">Desconto Progressivo!</p>
                    <p className="text-sm text-pink-600 mb-3">
                      Quanto mais produtos o cliente compra de você, maior o desconto! Isso cria <strong>clientes fiéis</strong>.
                    </p>
                    <p className="text-xs text-pink-500 mb-4 bg-white/50 p-2 rounded">
                      💰 <strong>1-2 produtos:</strong> Sem desconto
                      <br/>
                      💰 <strong>3-5 produtos:</strong> 5% de desconto
                      <br/>
                      💰 <strong>6+ produtos:</strong> 10% de desconto
                      <br/>
                      <strong>Resultado:</strong> +60% em clientes que compram múltiplos itens
                    </p>
                    <label className="flex items-center gap-3 cursor-pointer">
                      <input
                        type="checkbox"
                        checked={novoProduto.com_desconto_progressivo || false}
                        onChange={(e) => setNovoProduto({ ...novoProduto, com_desconto_progressivo: e.target.checked })}
                        className="w-5 h-5 text-pink-600 rounded"
                      />
                      <span className="text-sm font-bold text-pink-700">
                        ✅ Ativar Desconto Progressivo para este produto
                      </span>
                    </label>
                  </div>
                </div>
              </div>
            )}

            {/* SUGESTÃO 4: COMBO DE PRODUTOS */}
            {novoProduto.preco_anuncio && novoProduto.preco_minimo && parseInt(novoProduto.preco_anuncio) >= parseInt(novoProduto.preco_minimo) && (
              <div className="mb-6 bg-gradient-to-r from-purple-500/10 to-indigo-500/10 border-2 border-purple-400/50 rounded-xl p-5">
                <div className="flex items-start gap-4">
                  <div className="text-3xl">🎁</div>
                  <div className="flex-1">
                    <p className="font-bold text-purple-700 text-lg mb-2">Crie um Combo!</p>
                    <p className="text-sm text-purple-600 mb-3">
                      Combine este produto com outro e ofereça um <strong>super desconto</strong> para quem compra os 2!
                    </p>
                    <p className="text-xs text-purple-500 mb-4 bg-white/50 p-2 rounded">
                      📦 <strong>Exemplo:</strong> iPhone + Capinha = R$ 5.200
                      <br/>
                      📦 <strong>Sem combo:</strong> iPhone (R$ 5.000) + Capinha (R$ 300) = R$ 5.300
                      <br/>
                      <strong>Economia:</strong> R$ 100 de desconto (psicologia poderosa!)
                      <br/>
                      <strong>Resultado:</strong> +45% em vendas cruzadas
                    </p>
                    <label className="flex items-center gap-3 cursor-pointer">
                      <input
                        type="checkbox"
                        checked={novoProduto.permitir_combo || false}
                        onChange={(e) => setNovoProduto({ ...novoProduto, permitir_combo: e.target.checked })}
                        className="w-5 h-5 text-purple-600 rounded"
                      />
                      <span className="text-sm font-bold text-purple-700">
                        ✅ Este produto pode ser vendido em combo
                      </span>
                    </label>
                    {novoProduto.permitir_combo && (
                      <div className="mt-3 p-3 bg-purple-500/20 rounded border border-purple-400">
                        <input
                          type="text"
                          placeholder="Descrição do combo (ex: iPhone + Capinha)"
                          value={novoProduto.descricao_combo || ''}
                          onChange={(e) => setNovoProduto({ ...novoProduto, descricao_combo: e.target.value })}
                          className="w-full px-3 py-2 bg-slate-800/50 border border-purple-400/50 rounded text-white text-sm mb-2"
                        />
                        <input
                          type="number"
                          placeholder="Desconto do combo em R$ (ex: 100)"
                          value={novoProduto.desconto_combo || ''}
                          onChange={(e) => setNovoProduto({ ...novoProduto, desconto_combo: e.target.value })}
                          className="w-full px-3 py-2 bg-slate-800/50 border border-purple-400/50 rounded text-white text-sm"
                        />
                      </div>
                    )}
                  </div>
                </div>
              </div>
            )}

            {/* SUGESTÃO: REDUÇÃO AUTOMÁTICA */}
            {novoProduto.preco_anuncio && novoProduto.preco_minimo && parseInt(novoProduto.preco_anuncio) >= parseInt(novoProduto.preco_minimo) && (
              <div className="mb-6 bg-gradient-to-r from-green-500/10 to-emerald-500/10 border-2 border-green-400/50 rounded-xl p-5">
                <div className="flex items-start gap-4">
                  <div className="text-3xl">📉</div>
                  <div className="flex-1">
                    <p className="font-bold text-green-700 text-lg mb-2">Desconto = Mais Vendas!</p>
                    <p className="text-sm text-green-600 mb-3">
                      Quanto mais você reduz, mais vende! Ative redução automática de 1% por dia.
                    </p>
                    <p className="text-xs text-green-500 mb-4 bg-white/50 p-2 rounded">
                      ✅ Produtos com desconto vendem <strong>600% mais rápido</strong>
                      <br/>
                      ✅ Taxa de conversão: 35% (vs 5% sem desconto)
                      <br/>
                      ✅ Apareça em destaque com desconto ativo
                    </p>
                    <label className="flex items-center gap-3 cursor-pointer">
                      <input
                        type="checkbox"
                        checked={novoProduto.aceita_reducao_automatica || false}
                        onChange={(e) => setNovoProduto({ ...novoProduto, aceita_reducao_automatica: e.target.checked })}
                        className="w-5 h-5 text-green-600 rounded"
                      />
                      <span className="text-sm font-bold text-green-700">
                        ✅ Ativar redução automática de 1% por dia
                      </span>
                    </label>
                  </div>
                </div>
              </div>
            )}

            {/* VIANDAS/ALIMENTAÇÃO - CONFIGURAÇÃO ESPECIAL */}
            {novoProduto.eh_vianda && (
              <div className="mb-6 bg-gradient-to-r from-orange-500/10 to-red-500/10 border-2 border-orange-400/50 rounded-xl p-5">
                <div className="flex items-start gap-4">
                  <div className="text-3xl">🍽️</div>
                  <div className="flex-1">
                    <p className="font-bold text-orange-700 text-lg mb-4">Configuração de Vianda</p>
                    
                    {/* DIAS DA SEMANA */}
                    <div className="mb-4">
                      <p className="text-sm font-bold text-orange-700 mb-3">Qual(is) dia(s) da semana?</p>
                      <div className="grid grid-cols-2 md:grid-cols-4 gap-2">
                        {['Seg', 'Ter', 'Qua', 'Qui', 'Sex', 'Sab', 'Dom'].map((dia, idx) => (
                          <label key={dia} className="flex items-center gap-2 cursor-pointer bg-white/50 p-2 rounded hover:bg-white transition">
                            <input
                              type="checkbox"
                              checked={novoProduto.dias_semana?.includes(dia) || false}
                              onChange={(e) => {
                                const novosDias = e.target.checked
                                  ? [...(novoProduto.dias_semana || []), dia]
                                  : (novoProduto.dias_semana || []).filter(d => d !== dia);
                                setNovoProduto({ ...novoProduto, dias_semana: novosDias });
                              }}
                              className="w-4 h-4 text-orange-600 rounded"
                            />
                            <span className="text-sm font-bold text-orange-700">{dia}</span>
                          </label>
                        ))}
                      </div>
                    </div>

                    {/* QUANTIDADE POR DIA */}
                    <div className="mb-4">
                      <label className="block text-sm font-bold text-orange-700 mb-2">Quantidade disponível por dia</label>
                      <input
                        type="number"
                        placeholder="Ex: 10 marmitas"
                        value={novoProduto.quantidade_diaria || ''}
                        onChange={(e) => setNovoProduto({ ...novoProduto, quantidade_diaria: e.target.value })}
                        className="w-full px-4 py-2 border border-orange-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500"
                      />
                    </div>

                    <p className="text-xs text-orange-600 bg-white/50 p-2 rounded">
                      💡 Seu cardápio repebirá toda semana!
                      <br/>
                      📅 Clientes saberão exatamente quando você serve
                      <br/>
                      📊 Ajude a planejar a quantidade necessária
                    </p>
                  </div>
                </div>
              </div>
            )}

            {/* OPÇÕES DE ENTREGA */}
            <div className="mb-6">
              <div className="mb-4 p-4 bg-blue-50 rounded-lg border-l-4 border-blue-300">
                <p className="text-sm font-bold text-gray-700">📍 Você faz entrega?</p>
                <p className="text-xs text-gray-600 mt-1">Sua cidade: <strong>{usuarioLogado.cidade}</strong></p>
              </div>
              
              <div className="space-y-4">
                {/* OPÇÃO 1: NÃO ENTREGA */}
                <div className="border-2 border-gray-300 rounded-lg p-4 cursor-pointer hover:border-red-500 hover:bg-red-50 transition"
                  onClick={() => setNovoProduto({ ...novoProduto, faz_entrega: false })}>
                  <div className="flex items-center">
                    <input
                      type="radio"
                      name="entrega"
                      checked={novoProduto.faz_entrega === false}
                      onChange={() => setNovoProduto({ ...novoProduto, faz_entrega: false })}
                      className="w-4 h-4 text-green-600"
                    />
                    <span className="ml-4 font-bold text-gray-800">❌ Não entrego, retirar no local 📍</span>
                  </div>
                </div>

                {/* OPÇÃO 2: SIM, ENTREGA */}
                <div className="border-2 border-gray-300 rounded-lg p-4 cursor-pointer hover:border-green-500 hover:bg-green-50 transition"
                  onClick={() => setNovoProduto({ ...novoProduto, faz_entrega: true })}>
                  <div className="flex items-start">
                    <input
                      type="radio"
                      name="entrega"
                      checked={novoProduto.faz_entrega === true}
                      onChange={() => setNovoProduto({ ...novoProduto, faz_entrega: true })}
                      className="w-4 h-4 text-green-600 mt-1"
                    />
                    <div className="ml-4 flex-1">
                      <p className="font-bold text-gray-800 mb-4">✅ Sim, faço entrega 🚚</p>
                      
                      {novoProduto.faz_entrega === true && (
                        <div className="space-y-5 pt-4 border-t border-gray-300">
                          {/* OPÇÃO 1: GRÁTIS ATÉ X KM */}
                          <div className="bg-green-50 p-4 rounded-lg border-2 border-green-300">
                            <div className="flex items-center justify-between">
                              <label className="flex items-center cursor-pointer">
                                <input
                                  type="checkbox"
                                  checked={true}
                                  readOnly
                                  className="w-5 h-5 text-green-600 rounded"
                                />
                                <span className="ml-3 font-bold text-gray-800">✅ GRÁTIS até</span>
                              </label>
                              <div className="flex items-center gap-2">
                                <input
                                  type="number"
                                  placeholder="100"
                                  value={novoProduto.raio_entrega}
                                  onChange={(e) => setNovoProduto({ ...novoProduto, raio_entrega: e.target.value })}
                                  className="w-20 px-2 py-1 border border-green-400 rounded focus:outline-none focus:ring-2 focus:ring-green-600 text-center font-bold"
                                />
                                <span className="font-bold text-gray-800">km</span>
                              </div>
                            </div>
                            <p className="text-xs text-gray-600 ml-8 mt-2">A partir de {usuarioLogado.cidade}</p>
                          </div>

                          {/* OPÇÃO 2: APÓS X KM, COBRAR */}
                          <div className="bg-orange-50 p-4 rounded-lg border-2 border-orange-300">
                            <label className="flex items-center cursor-pointer mb-3">
                              <input
                                type="checkbox"
                                checked={novoProduto.valor_frete !== '0'}
                                onChange={() => {
                                  if (novoProduto.valor_frete === '0') {
                                    setNovoProduto({ ...novoProduto, valor_frete: '50' });
                                  } else {
                                    setNovoProduto({ ...novoProduto, valor_frete: '0' });
                                  }
                                }}
                                className="w-5 h-5 text-orange-600 rounded"
                              />
                              <span className="ml-3 font-bold text-gray-800">📦 APÓS o raio grátis, cobrar frete</span>
                            </label>

                            {novoProduto.valor_frete !== '0' && (
                              <div className="ml-8 space-y-3">
                                <div>
                                  <p className="text-xs text-gray-600 mb-2">Acima de {novoProduto.raio_entrega}km, cobrar:</p>
                                  <div className="flex items-center gap-2">
                                    <span className="text-sm font-bold text-gray-800">R$</span>
                                    <input
                                      type="number"
                                      placeholder="50"
                                      value={novoProduto.valor_frete}
                                      onChange={(e) => setNovoProduto({ ...novoProduto, valor_frete: e.target.value })}
                                      className="w-20 px-2 py-1 border border-orange-400 rounded focus:outline-none focus:ring-2 focus:ring-orange-600 text-center font-bold"
                                    />
                                    <span className="text-sm font-bold text-gray-800">por km adicional</span>
                                  </div>
                                </div>

                                <div>
                                  <p className="text-xs text-gray-600 mb-2">Raio máximo de entrega:</p>
                                  <div className="flex items-center gap-2">
                                    <span className="text-sm font-bold text-gray-800">Até:</span>
                                    <input
                                      type="number"
                                      placeholder="200"
                                      value={novoProduto.preco_minimo_frete || ''}
                                      onChange={(e) => setNovoProduto({ ...novoProduto, preco_minimo_frete: e.target.value })}
                                      className="w-20 px-2 py-1 border border-orange-400 rounded focus:outline-none focus:ring-2 focus:ring-orange-600 text-center font-bold"
                                    />
                                    <span className="text-sm font-bold text-gray-800">km</span>
                                  </div>
                                </div>
                              </div>
                            )}
                          </div>

                          {/* RESUMO */}
                          <div className="bg-blue-100 p-3 rounded-lg border-l-4 border-blue-500">
                            <p className="text-sm font-bold text-blue-900">📋 Resumo da Entrega:</p>
                            {novoProduto.valor_frete === '0' ? (
                              <p className="text-xs text-blue-800 mt-1">✅ Entrega GRÁTIS até {novoProduto.raio_entrega}km de {usuarioLogado.cidade}</p>
                            ) : (
                              <>
                                <p className="text-xs text-blue-800 mt-1">✅ GRÁTIS até {novoProduto.raio_entrega}km</p>
                                <p className="text-xs text-blue-800">📦 Após {novoProduto.raio_entrega}km, R$ {novoProduto.valor_frete}/km até {novoProduto.preco_minimo_frete || '?'}km</p>
                              </>
                            )}
                          </div>
                        </div>
                      )}
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <div className="flex gap-4">
              <button
                onClick={produtoEditando ? salvarEdicao : adicionarProduto}
                className="flex-1 bg-green-600 hover:bg-green-700 text-white font-bold py-3 rounded-lg transition"
              >
                {produtoEditando ? '💾 Salvar Alterações' : '📤 Publicar Produto'}
              </button>
              <button
                onClick={produtoEditando ? cancelarEdicao : () => setTela('meus-produtos')}
                className="flex-1 bg-gray-400 hover:bg-gray-500 text-white font-bold py-3 rounded-lg transition"
              >
                Cancelar
              </button>
            </div>
          </div>
        </div>
      </div>
    );
  }

  // MEUS PRODUTOS
  if (tela === 'meus-produtos') {
    return (
      <div className="min-h-screen bg-gray-100">
        {navbar}

        <div className="max-w-7xl mx-auto p-6">
          <button
            onClick={() => setTela('inicio')}
            className="bg-gray-600 hover:bg-gray-700 text-white font-bold px-6 py-3 rounded-lg mb-6 transition"
          >
            ← Voltar
          </button>

          <div className="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
            <div className="bg-white rounded-lg shadow-lg p-6">
              <p className="text-gray-600 text-sm">Total de Produtos</p>
              <p className="text-4xl font-bold text-green-600">{meusProdutos.length}</p>
            </div>
            <div className="bg-white rounded-lg shadow-lg p-6">
              <p className="text-gray-600 text-sm">Visualizações Totais</p>
              <p className="text-4xl font-bold text-purple-600">
                {meusProdutos.reduce((acc, p) => acc + p.visualizacoes, 0)}
              </p>
            </div>
          </div>

          <button
            onClick={() => setTela('novo-produto')}
            className="bg-green-600 hover:bg-green-700 text-white font-bold px-6 py-3 rounded-lg mb-6 transition flex items-center gap-2"
          >
            <Plus size={20} /> Novo Produto
          </button>

          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            {meusProdutos.map(p => (
              <div key={p.id} className={`bg-white rounded-lg shadow-lg overflow-hidden relative ${negociacoesAtivas[p.id] ? 'ring-4 ring-yellow-400' : ''}`} style={negociacoesAtivas[p.id] ? { animation: 'pulse 1.5s cubic-bezier(0.4, 0, 0.6, 1) infinite' } : {}}>
                {negociacoesAtivas[p.id] && (
                  <div className="absolute top-2 right-2 z-10">
                    <div className="relative">
                      <div className="absolute inset-0 bg-yellow-400 rounded-full animate-ping opacity-75"></div>
                      <div className="relative px-2 py-1 bg-yellow-400 text-yellow-900 rounded-full font-bold text-xs shadow-lg">
                        🔴 Em Negociação
                      </div>
                    </div>
                  </div>
                )}

                {p.fotos && p.fotos.length > 0 ? (
                  <img src={p.fotos[0]} alt={p.nome} className="w-full h-40 object-cover" />
                ) : (
                  <div className="bg-gradient-to-br from-blue-400 to-blue-500 h-40 flex items-center justify-center text-5xl">
                    {p.emoji}
                  </div>
                )}

                <div className="p-4">
                  <h3 className="font-bold text-lg mb-2">{p.nome}</h3>
                  <div className="flex gap-2 mb-3">
                    <span className="text-xs bg-purple-100 text-purple-700 px-2 py-1 rounded">📦 {p.categoria}</span>
                    <span className="text-xs bg-blue-100 text-blue-700 px-2 py-1 rounded">📍 {p.estado}</span>
                  </div>
                  <div className="space-y-2 mb-4 bg-gray-50 p-3 rounded-lg text-sm">
                    {p.aceita_reducao_automatica ? (
                      <>
                        <p><span className="font-bold">Anúncio original:</span> R$ {p.preco_anuncio.toLocaleString('pt-BR')}</p>
                        <p><span className="font-bold">Anúncio agora:</span> R$ {calcularPrecoComReducao(p).preco_anuncio_atual.toLocaleString('pt-BR')} <span className="text-green-600">📉</span></p>
                        <p><span className="font-bold">Mínimo:</span> R$ {calcularPrecoComReducao(p).preco_minimo_atual.toLocaleString('pt-BR')}</p>
                        <p className="text-green-700 font-bold">✅ Redução automática: -{calcularPrecoComReducao(p).dias_reducao}%</p>
                      </>
                    ) : (
                      <>
                        <p><span className="font-bold">Anúncio:</span> R$ {p.preco_anuncio.toLocaleString('pt-BR')}</p>
                        <p><span className="font-bold">Mínimo:</span> R$ {p.preco_minimo.toLocaleString('pt-BR')}</p>
                      </>
                    )}
                  </div>

                  <div className="mb-3 bg-blue-50 p-3 rounded-lg text-xs border-l-4 border-blue-300">
                    {p.eh_vianda ? (
                      <div className="space-y-2">
                        <p className="font-semibold text-orange-900 bg-orange-100 px-2 py-1 rounded">🍽️ VIANDA RECORRENTE</p>
                        <p className="text-orange-800">📅 Dias: {p.dias_semana?.join(', ') || 'Não definido'}</p>
                        <p className="text-orange-800">📦 Quantidade: {p.quantidade_diaria || 0} unidades/dia</p>
                        <p className="text-orange-700 text-xs italic">
                          💡 Seu cardápio repete toda semana na plataforma
                        </p>
                      </div>
                    ) : p.faz_entrega ? (
                      <div className="space-y-1">
                        <p className="font-semibold text-blue-900">🚚 ENTREGA</p>
                        {p.valor_frete === 0 ? (
                          <>
                            <p className="text-blue-800 font-bold bg-green-100 px-2 py-1 rounded">✅ FRETE GRÁTIS ATÉ {p.raio_entrega}KM</p>
                            <p className="text-blue-800">📍 {p.cidade_vendedor}</p>
                          </>
                        ) : (
                          <>
                            <p className="text-blue-800">├─ ✅ GRÁTIS até {p.raio_entrega}km</p>
                            <p className="text-blue-800">└─ 📦 R$ {(p.valor_frete || 0).toLocaleString('pt-BR')} acima de {p.raio_entrega}km</p>
                            <p className="text-blue-800">📍 {p.cidade_vendedor}</p>
                          </>
                        )}
                        {p.permitir_combo && (
                          <p className="text-purple-700 font-bold bg-purple-100 px-2 py-1 rounded mt-1">
                            🎁 COMBO: {p.descricao_combo} (-R$ {p.desconto_combo})
                          </p>
                        )}
                      </div>
                    ) : (
                      <div className="space-y-1">
                        <p className="font-semibold text-blue-900">📍 ENTREGA</p>
                        <p className="text-blue-800">├─ Retira no local</p>
                        <p className="text-blue-800">└─ 📍 {p.cidade_vendedor}</p>
                      </div>
                    )}
                  </div>

                  <div className="text-xs text-gray-600 mb-4">
                    <p>👁️ {p.visualizacoes} visualizações</p>
                    <p>💬 {p.em_negociacao} em negociação</p>
                    {p.proxima_renovacao && (
                      <p className="text-green-600 font-bold mt-2">⏰ {formatarRenovacao(p.proxima_renovacao)}</p>
                    )}
                  </div>

                  <div className="flex gap-2">
                    <button
                      onClick={() => {
                        abrirEdicao(p);
                        setTela('editar-produto');
                      }}
                      className="flex-1 bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 rounded-lg text-sm transition"
                    >
                      ✏️ Editar
                    </button>
                    <button
                      onClick={() => reduzirPreco(p.id)}
                      className="flex-1 bg-yellow-500 hover:bg-yellow-600 text-white font-bold py-2 rounded-lg text-sm transition"
                    >
                      ↓ Reduzir
                    </button>
                    <button
                      onClick={() => setProdutos(produtos.filter(x => x.id !== p.id))}
                      className="flex-1 bg-red-500 hover:bg-red-600 text-white font-bold py-2 rounded-lg text-sm transition"
                    >
                      🗑️ Remover
                    </button>
                  </div>
                </div>
              </div>
            ))}
          </div>
        </div>
      </div>
    );
  }

  // TELA INICIAL
  if (tela === 'inicio') {
    const produtosOrdenados = [...produtos].sort((a, b) => b.dataCriacao - a.dataCriacao).filter(p => p.status === 'ativo');
    
    const produtosFiltrados = produtosOrdenados.filter(p => {
      const temCategoria = filtroCategoria === 'Todos' || p.categoria === filtroCategoria;
      const temEstado = filtroCidade === 'Todas' || p.estado === filtroCidade;
      const temBusca = p.nome.toLowerCase().includes(busca.toLowerCase());
      
      // FILTRO DE TEMPO
      let temFiltroTempo = true;
      if (filtroTempo !== 'todos') {
        const categoria = categorizarPorTempo(p.dataCriacao);
        temFiltroTempo = categoria === filtroTempo;
      }
      
      return temCategoria && temEstado && temBusca && temFiltroTempo;
    });

    return (
      <div className="min-h-screen bg-gradient-to-br from-slate-950 via-purple-950 to-slate-950">
        {navbar}

        <div className="max-w-7xl mx-auto p-6">
          {/* BANNER - USUÁRIO TESTE */}
          {usuarioLogado?.email === 'teste@test.com' && (
            <div className="mb-8 bg-gradient-to-r from-purple-500/20 to-cyan-500/20 border-2 border-purple-400 rounded-xl p-6 backdrop-blur-sm">
              <div className="flex items-start gap-4">
                <div className="text-4xl">🧪</div>
                <div className="flex-1">
                  <h3 className="text-2xl font-black text-cyan-300 mb-2">Bem-vindo ao Teste Completo!</h3>
                  <p className="text-purple-200 mb-3">
                    Você está logado com a conta de TESTE. Nesta conta você pode:
                  </p>
                  <ul className="grid grid-cols-1 md:grid-cols-2 gap-2 text-sm text-purple-100 mb-4">
                    <li>✅ Criar novos produtos</li>
                    <li>✅ Editar e deletar produtos existentes</li>
                    <li>✅ Testar todas as categorias</li>
                    <li>✅ Testar redução automática</li>
                    <li>✅ Testar viandas com calendário</li>
                    <li>✅ Testar sistema de negociação</li>
                    <li>✅ Testar combos e sugestões</li>
                    <li>✅ Visualizar como comprador</li>
                  </ul>
                  <p className="text-xs text-purple-300 bg-purple-950/50 p-2 rounded border border-purple-400/30">
                    💡 Existem 5 produtos de exemplo já criados. Edite, delete ou crie novos à vontade!
                  </p>
                </div>
              </div>
            </div>
          )}

          {usuarioLogado && (
            <div className="mb-8 flex gap-4">
              <button
                onClick={() => setTela('meus-produtos')}
                className="bg-gradient-to-r from-blue-600 to-cyan-600 hover:from-blue-500 hover:to-cyan-500 text-white font-bold px-8 py-3.5 rounded-xl transition-all duration-300 shadow-lg hover:shadow-cyan-500/50 border border-cyan-300/30 flex items-center gap-2 uppercase tracking-wider text-sm"
              >
                📦 Meus Produtos
              </button>
              <button
                onClick={() => setTela('novo-produto')}
                className="bg-gradient-to-r from-green-600 to-emerald-600 hover:from-green-500 hover:to-emerald-500 text-white font-bold px-8 py-3.5 rounded-xl transition-all duration-300 shadow-lg hover:shadow-green-500/50 border border-green-300/30 flex items-center gap-2 uppercase tracking-wider text-sm"
              >
                <Plus size={20} /> Vender
              </button>
            </div>
          )}

          <div className="mb-8">
            <h2 className="text-5xl font-black mb-3 bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-400 bg-clip-text text-transparent drop-shadow-lg">
              🛍️ Produtos Disponíveis
            </h2>
            <p className="text-purple-300 font-medium text-lg">Encontre os melhores produtos de nossa comunidade</p>
          </div>

          {/* FILTROS */}
          <div className="bg-slate-900/80 backdrop-blur-xl rounded-2xl shadow-2xl p-6 mb-8 border border-purple-500/20">
            {/* ABAS DE TEMPO - GRID PROFISSIONAL */}
            <div className="mb-8">
              <p className="text-sm font-bold text-cyan-400 mb-4 uppercase tracking-wider">📊 Filtrar por Status</p>
              <div className="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-3">
                <button
                  onClick={() => setFiltroTempo('todos')}
                  className={`px-4 py-3 rounded-xl font-bold transition-all text-center ${
                    filtroTempo === 'todos'
                      ? 'bg-cyan-600 text-white shadow-lg shadow-cyan-500/50 border border-cyan-400'
                      : 'bg-slate-800/50 text-cyan-400 border border-cyan-400/30 hover:border-cyan-400 hover:bg-slate-800'
                  }`}
                >
                  <div className="text-lg mb-1">📦</div>
                  <div className="text-xs">Todos</div>
                </button>
                <button
                  onClick={() => setFiltroTempo('novo')}
                  className={`px-4 py-3 rounded-xl font-bold transition-all text-center ${
                    filtroTempo === 'novo'
                      ? 'bg-yellow-600 text-white shadow-lg shadow-yellow-500/50 border border-yellow-400'
                      : 'bg-slate-800/50 text-yellow-400 border border-yellow-400/30 hover:border-yellow-400 hover:bg-slate-800'
                  }`}
                >
                  <div className="text-lg mb-1">🌟</div>
                  <div className="text-xs">Acabei de Publicar</div>
                </button>
                <button
                  onClick={() => setFiltroTempo('rapido')}
                  className={`px-4 py-3 rounded-xl font-bold transition-all text-center ${
                    filtroTempo === 'rapido'
                      ? 'bg-blue-600 text-white shadow-lg shadow-blue-500/50 border border-blue-400'
                      : 'bg-slate-800/50 text-blue-400 border border-blue-400/30 hover:border-blue-400 hover:bg-slate-800'
                  }`}
                >
                  <div className="text-lg mb-1">⚡</div>
                  <div className="text-xs">Rápido</div>
                </button>
                <button
                  onClick={() => setFiltroTempo('vendendo')}
                  className={`px-4 py-3 rounded-xl font-bold transition-all text-center ${
                    filtroTempo === 'vendendo'
                      ? 'bg-green-600 text-white shadow-lg shadow-green-500/50 border border-green-400'
                      : 'bg-slate-800/50 text-green-400 border border-green-400/30 hover:border-green-400 hover:bg-slate-800'
                  }`}
                >
                  <div className="text-lg mb-1">📦</div>
                  <div className="text-xs">Vendendo Agora</div>
                </button>
                <button
                  onClick={() => setFiltroTempo('liquidando')}
                  className={`px-4 py-3 rounded-xl font-bold transition-all text-center ${
                    filtroTempo === 'liquidando'
                      ? 'bg-red-600 text-white shadow-lg shadow-red-500/50 border border-red-400'
                      : 'bg-slate-800/50 text-red-400 border border-red-400/30 hover:border-red-400 hover:bg-slate-800'
                  }`}
                >
                  <div className="text-lg mb-1">🔥</div>
                  <div className="text-xs">Liquidando</div>
                </button>
                <button
                  onClick={() => setFiltroTempo('saldao')}
                  className={`px-4 py-3 rounded-xl font-bold transition-all text-center ${
                    filtroTempo === 'saldao'
                      ? 'bg-pink-600 text-white shadow-lg shadow-pink-500/50 border border-pink-400'
                      : 'bg-slate-800/50 text-pink-400 border border-pink-400/30 hover:border-pink-400 hover:bg-slate-800'
                  }`}
                >
                  <div className="text-lg mb-1">💰</div>
                  <div className="text-xs">Saldão</div>
                </button>
              </div>
            </div>

            {/* BUSCA E FILTROS NORMAIS */}
            <div className="mb-5">
              <input
                type="text"
                placeholder="🔍 Buscar produtos..."
                value={busca}
                onChange={(e) => setBusca(e.target.value)}
                className="w-full px-5 py-3.5 bg-slate-800/50 border-2 border-purple-500/30 rounded-xl focus:outline-none focus:border-cyan-400 focus:shadow-lg focus:shadow-cyan-500/20 transition-all text-white placeholder-purple-400/60 font-medium"
              />
            </div>

            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div>
                <label className="block text-sm font-bold text-cyan-400 mb-3 uppercase tracking-wider">📁 Categoria</label>
                <select
                  value={filtroCategoria}
                  onChange={(e) => setFiltroCategoria(e.target.value)}
                  className="w-full px-4 py-2.5 bg-slate-800/50 border-2 border-purple-500/30 rounded-xl focus:outline-none focus:border-cyan-400 text-white font-medium cursor-pointer hover:border-purple-400/50 transition-all"
                >
                  <option value="Todos">Todos</option>
                  <option value="Eletrônicos">Eletrônicos</option>
                  <option value="Moda">Moda</option>
                  <option value="Casa">Casa</option>
                  <option value="Esportes">Esportes</option>
                  <option value="Livros">Livros</option>
                  <option value="Outros">Outros</option>
                </select>
              </div>

              <div>
                <label className="block text-sm font-bold text-pink-400 mb-3 uppercase tracking-wider">🗺️ Estado</label>
                <select
                  value={filtroCidade}
                  onChange={(e) => setFiltroCidade(e.target.value)}
                  className="w-full px-4 py-2.5 bg-slate-800/50 border-2 border-purple-500/30 rounded-xl focus:outline-none focus:border-pink-400 text-white font-medium cursor-pointer hover:border-purple-400/50 transition-all"
                >
                  <option value="Todas">Todos os Estados</option>
                  <option value="SP">São Paulo</option>
                  <option value="RJ">Rio de Janeiro</option>
                  <option value="MG">Minas Gerais</option>
                  <option value="BA">Bahia</option>
                  <option value="SC">Santa Catarina</option>
                  <option value="RS">Rio Grande do Sul</option>
                  <option value="PR">Paraná</option>
                  <option value="DF">Distrito Federal</option>
                </select>
              </div>
            </div>
          </div>

          {produtosFiltrados.length === 0 ? (
            <div className="text-center py-12">
              <p className="text-2xl text-gray-500">Nenhum produto encontrado</p>
            </div>
          ) : (
            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
              {produtosFiltrados.map(p => (
                <div key={p.id} className={`bg-white rounded-lg shadow-lg overflow-hidden hover:shadow-xl transition relative ${negociacoesAtivas[p.id] ? 'ring-4 ring-yellow-400' : ''}`} style={negociacoesAtivas[p.id] ? { animation: 'pulse 1.5s cubic-bezier(0.4, 0, 0.6, 1) infinite' } : {}}>
                  {negociacoesAtivas[p.id] && (
                    <div className="absolute top-3 right-3 z-10">
                      <div className="relative">
                        <div className="absolute inset-0 bg-yellow-400 rounded-full animate-ping opacity-75"></div>
                        <div className="relative px-3 py-1 bg-yellow-400 text-yellow-900 rounded-full font-bold text-xs shadow-lg">
                          ✅ Vendedor Negocia!
                        </div>
                      </div>
                    </div>
                  )}

                  {p.fotos && p.fotos.length > 0 ? (
                    <img src={p.fotos[0]} alt={p.nome} className="w-full h-40 object-cover" />
                  ) : (
                    <div className="bg-gradient-to-br from-green-400 to-emerald-500 h-40 flex items-center justify-center text-6xl">
                      {p.emoji}
                    </div>
                  )}

                  <div className="p-6">
                    <h3 className="font-bold text-lg text-gray-900 mb-2">{p.nome}</h3>
                    <div className="flex gap-2 mb-2">
                      <span className="text-xs bg-purple-100 text-purple-700 px-2 py-1 rounded">📦 {p.categoria}</span>
                      <span className="text-xs bg-blue-100 text-blue-700 px-2 py-1 rounded">📍 {p.estado}</span>
                      
                      {/* BADGE DE TEMPO */}
                      {categorizarPorTempo(p.dataCriacao) === 'novo' && (
                        <span className="text-xs bg-yellow-100 text-yellow-700 px-2 py-1 rounded font-bold">✨ Novo!</span>
                      )}
                      {categorizarPorTempo(p.dataCriacao) === 'rapido' && (
                        <span className="text-xs bg-blue-100 text-blue-700 px-2 py-1 rounded font-bold">⚡ Rápido!</span>
                      )}
                      {categorizarPorTempo(p.dataCriacao) === 'vendendo' && (
                        <span className="text-xs bg-green-100 text-green-700 px-2 py-1 rounded font-bold">📦 Vendendo!</span>
                      )}
                      {categorizarPorTempo(p.dataCriacao) === 'liquidando' && (
                        <span className="text-xs bg-red-100 text-red-700 px-2 py-1 rounded font-bold">🔥 Liquidando!</span>
                      )}
                      {categorizarPorTempo(p.dataCriacao) === 'saldao' && (
                        <span className="text-xs bg-pink-100 text-pink-700 px-2 py-1 rounded font-bold">💰 Saldão!</span>
                      )}
                      {p.aceita_reducao_automatica && (
                        <span className="text-xs bg-green-100 text-green-700 px-2 py-1 rounded font-bold animate-pulse">📉 Preço Caindo!</span>
                      )}
                    </div>
                    <p className="text-gray-600 text-sm mb-4">Vendedor: {p.vendedorNome}</p>

                    {/* VIANDA - EXIBIR DIAS E QUANTIDADE */}
                    {p.eh_vianda && (
                      <div className="mb-4 bg-orange-50 p-3 rounded-lg border-l-4 border-orange-400">
                        <p className="text-xs font-bold text-orange-700 mb-2">📅 Disponível:</p>
                        <div className="flex flex-wrap gap-1 mb-2">
                          {p.dias_semana && p.dias_semana.map(dia => (
                            <span key={dia} className="text-xs bg-orange-200 text-orange-800 px-2 py-1 rounded font-bold">
                              {dia}
                            </span>
                          ))}
                        </div>
                        {p.quantidade_diaria > 0 && (
                          <p className="text-xs text-orange-700">
                            📦 {p.quantidade_diaria} {p.quantidade_diaria === 1 ? 'unidade' : 'unidades'} por dia
                          </p>
                        )}
                      </div>
                    )}

                    <div className="mb-4 bg-gray-100 p-3 rounded-lg">
                      {p.aceita_reducao_automatica ? (
                        <>
                          <p className="text-xs text-gray-600 mb-1">Preço original: R$ {p.preco_anuncio.toLocaleString('pt-BR')}</p>
                          <p className="text-2xl font-bold text-green-600">
                            R$ {calcularPrecoComReducao(p).preco_anuncio_atual.toLocaleString('pt-BR')} 
                            <span className="text-xs text-green-500 ml-2">📉 -1% ao dia</span>
                          </p>
                        </>
                      ) : (
                        <p className="text-2xl font-bold text-green-600">
                          R$ {p.preco_anuncio.toLocaleString('pt-BR')}
                        </p>
                      )}
                      <p className="text-xs text-gray-600 mt-1">
                        👁️ {p.visualizacoes} visualizações | 💬 {p.em_negociacao} em negociação
                      </p>
                    </div>

                    {/* Informações de Entrega */}
                    <div className="mb-4 bg-blue-50 p-3 rounded-lg text-sm border-l-4 border-blue-300">
                      {p.faz_entrega ? (
                        <div className="space-y-1">
                          <p className="font-semibold text-blue-900">🚚 ENTREGA</p>
                          {p.valor_frete === 0 ? (
                            <>
                              <p className="text-blue-800 text-xs font-bold bg-green-100 px-2 py-1 rounded">✅ FRETE GRÁTIS ATÉ {p.raio_entrega}KM</p>
                              <p className="text-blue-800 text-xs">📍 {p.cidade_vendedor}</p>
                            </>
                          ) : (
                            <>
                              <p className="text-blue-800 text-xs">├─ ✅ GRÁTIS até {p.raio_entrega}km</p>
                              <p className="text-blue-800 text-xs">└─ 📦 R$ {(p.valor_frete || 0).toLocaleString('pt-BR')} acima de {p.raio_entrega}km</p>
                              <p className="text-blue-800 text-xs">📍 {p.cidade_vendedor}</p>
                            </>
                          )}
                          {p.permitir_combo && (
                            <p className="text-purple-700 text-xs font-bold bg-purple-100 px-2 py-1 rounded mt-1">
                              🎁 COMBO: {p.descricao_combo} (-R$ {p.desconto_combo})
                            </p>
                          )}
                        </div>
                      ) : (
                        <div className="space-y-1">
                          <p className="font-semibold text-blue-900">📍 ENTREGA</p>
                          <p className="text-blue-800 text-xs">├─ Retira no local</p>
                          <p className="text-blue-800 text-xs">└─ 📍 {p.cidade_vendedor}</p>
                        </div>
                      )}
                    </div>

                    <button
                      onClick={() => abrirChat(p)}
                      disabled={!podeNegociar(p)}
                      className={`w-full font-bold py-3 rounded-lg transition ${
                        podeNegociar(p)
                          ? 'bg-gradient-to-r from-green-600 to-emerald-600 hover:from-green-500 hover:to-emerald-500 text-white shadow-lg hover:shadow-green-500/50 border border-green-300/30 cursor-pointer'
                          : 'bg-gray-400 text-gray-600 cursor-not-allowed opacity-60'
                      }`}
                      title={!podeNegociar(p) ? motivoBloqueio(p) : 'Clique para negociar'}
                    >
                      {podeNegociar(p) ? '💬 Negociar' : '🔒 Bloqueado'}
                    </button>

                    {!usuarioLogado && (
                      <p className="text-xs text-purple-400 text-center mt-3 bg-purple-500/10 p-2 rounded">
                        Faça login para negociar
                      </p>
                    )}

                    {usuarioLogado && !podeNegociar(p) && (
                      <p className="text-xs text-red-400 text-center mt-3 bg-red-500/10 p-2 rounded border border-red-400/20">
                        ⚠️ {motivoBloqueio(p)}
                      </p>
                    )}
                  </div>
                </div>
              ))}
            </div>
          )}
        </div>
      </div>
    );
  }

  // CHAT
  // CHAT DE NEGOCIAÇÃO
  if (chatAberto) {
    // VERIFICAÇÃO DE SEGURANÇA - FECHAR CHAT SE FOR DETECTADO BLOQUEIO
    if (!podeNegociar(chatAberto)) {
      return (
        <div className="fixed inset-0 bg-black/60 backdrop-blur-sm flex items-center justify-center p-4 z-50">
          <div className="w-full max-w-md bg-red-900/95 rounded-2xl shadow-2xl p-8 border-2 border-red-500 text-center">
            <div className="text-5xl mb-4">🔒</div>
            <h3 className="text-2xl font-bold text-red-300 mb-3">Acesso Bloqueado</h3>
            <p className="text-red-200 mb-4 text-sm">
              {motivoBloqueio(chatAberto)}
            </p>
            <p className="text-xs text-red-400 mb-6 bg-red-500/20 p-3 rounded">
              ⚠️ Sistema de segurança: Detectamos que você está na mesma rede (IP: {usuarioIP}) do vendedor.
            </p>
            <button
              onClick={fecharChat}
              className="w-full bg-gradient-to-r from-red-600 to-red-700 hover:from-red-500 hover:to-red-600 text-white font-bold py-3 rounded-lg transition"
            >
              ✅ Fechar
            </button>
          </div>
        </div>
      );
    }

    return (
      <div className="fixed inset-0 bg-black/60 backdrop-blur-sm flex items-center justify-center p-4 z-50">
        <div className="w-full max-w-2xl bg-slate-900/95 rounded-2xl shadow-2xl flex flex-col h-[600px] border border-purple-500/30">
          {/* HEADER */}
          <div className="bg-gradient-to-r from-slate-900 via-purple-900 to-slate-900 text-white p-5 flex justify-between items-center border-b border-purple-500/30">
            <div>
              <h3 className="font-bold text-lg text-cyan-400">{chatAberto.emoji} {chatAberto.nome}</h3>
              <p className="text-xs text-purple-300">💼 {chatAberto.vendedorNome} • 📍 {chatAberto.cidade_vendedor}</p>
            </div>
            <button onClick={fecharChat} className="hover:bg-red-500/20 p-2 rounded-lg transition">
              <X size={24} className="text-red-400" />
            </button>
          </div>

          {/* INFO PREÇO */}
          <div className="bg-purple-500/10 border-b border-purple-500/20 px-5 py-3">
            <div className="flex justify-between items-center">
              <div>
                <p className="text-xs text-purple-300">📊 Preço Anúncio</p>
                <p className="text-xl font-bold text-cyan-400">R$ {chatAberto.preco_anuncio.toLocaleString('pt-BR')}</p>
              </div>
              <div className="text-right">
                <p className="text-xs text-purple-300">🔒 Preço Mínimo</p>
                <p className="text-xl font-bold text-green-400">R$ {chatAberto.preco_minimo.toLocaleString('pt-BR')}</p>
              </div>
              <div className="text-right">
                <p className="text-xs text-purple-300">🚚 Frete</p>
                <p className="text-xl font-bold text-pink-400">✅ GRÁTIS</p>
              </div>
            </div>
          </div>

          {/* CHAT */}
          <div className="flex-1 overflow-y-auto p-5 space-y-4 bg-gradient-to-b from-slate-900 to-slate-950">
            {mensagens.length === 0 ? (
              <div className="text-center py-8 text-purple-400">
                <p className="text-sm">💬 Comece a negociar!</p>
              </div>
            ) : (
              mensagens.map(m => (
                <div key={m.id} className={`flex ${m.tipo === 'comprador' ? 'justify-end' : 'justify-start'}`}>
                  <div
                    className={`max-w-xs px-4 py-3 rounded-xl text-sm font-medium ${
                      m.tipo === 'comprador'
                        ? 'bg-gradient-to-r from-cyan-500 to-blue-600 text-white rounded-br-none'
                        : m.tipo === 'sistema'
                        ? 'bg-yellow-500/20 text-yellow-300 border border-yellow-500/30'
                        : 'bg-slate-800 text-purple-300 border border-purple-500/30 rounded-bl-none'
                    }`}
                  >
                    {m.texto}
                  </div>
                </div>
              ))
            )}
          </div>

          {/* INPUT */}
          <div className="border-t border-purple-500/30 bg-slate-900 p-4">
            <div className="flex gap-3">
              <input
                type="text"
                placeholder="💰 Digite sua oferta em reais... (ex: 2500)"
                value={inputChat}
                onChange={(e) => setInputChat(e.target.value)}
                onKeyPress={(e) => e.key === 'Enter' && enviarMensagem()}
                className="flex-1 px-4 py-3 bg-slate-800/50 border-2 border-purple-500/30 rounded-xl text-white placeholder-purple-400/60 focus:outline-none focus:border-cyan-400 focus:shadow-lg focus:shadow-cyan-500/20 transition-all font-medium"
              />
              <button
                onClick={enviarMensagem}
                className="bg-gradient-to-r from-cyan-500 to-blue-600 hover:from-cyan-400 hover:to-blue-500 text-white px-6 py-3 rounded-xl transition-all duration-300 shadow-lg hover:shadow-cyan-500/50 border border-cyan-300/30 font-bold"
              >
                <Send size={20} />
              </button>
            </div>
            <p className="text-xs text-purple-400 mt-2">
              ⚠️ Digite um número (exemplo: 2500) ou palavras como "compro", "topei" para fechar!
            </p>
          </div>
        </div>
      </div>
    );
  }
};

export default Marketplace;
