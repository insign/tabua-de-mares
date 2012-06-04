<?php

  /**
   * @since 29/05/2012
   * @author Hélio A. de Oliveira <insign@gmail.com>
   * @HELIO
   */
  include_once 'Snoopy.class.php';

  class tabua_mare {

	  protected $snoopy;
	  public $ano, $mes, $id_tabua = '30149', $cache = true, $cache_dirname = 'cache';
	  protected $regex_1;
	  protected $regex_2;

	  function __construct() {
		  $this -> snoopy = new Snoopy;
		  $meses = array( 1 => "Janeiro", 2 => "Fevereiro", 3 => "Março", 4 => "Abril", 5 => "Maio", 6 => "Junho", 7 => "Julho", 8 => "Agosto", 9 => "Setembro", 10 => "Outubro", 11 => "Novembro", 12 => "Dezembro" );

		  $this -> ano = date( 'Y' );
		  $this -> mes = substr( $meses[ date( 'n' ) ], 0, 3 );
		  $this -> regex_1 = '%(\d\d/' . date( 'm/Y' ) . ')(.+?)(?:COLSPAN|TABLE)%simx';
		  $this -> regex_2 = '/(\d{2}:\d{2}).+?\s(\d\.\d)/simx';
	  }

	  function fetch( $URI ) {
		  $this -> snoopy -> fetch( $URI );
		  return $this -> snoopy -> results;
	  }

	  function get( $URI = null ) {
		  if ( !$URI ) {
			  $URI = "http://www.mar.mil.br/dhn/chm/tabuas/{$this -> id_tabua }{$this -> mes }{$this -> ano }.htm";
		  }

		  if ( $this -> cache ) {
			  $sha1 = sha1( $URI );

			  $file_cache_path = dirname(__FILE__)."/{$this -> cache_dirname}/$sha1";

			  if ( file_exists( $file_cache_path ) ) {
				  $results =  file_get_contents($file_cache_path);
			  } else {
				  $results = $this -> fetch( $URI );
				  file_put_contents($file_cache_path, $results);
			  }
		  } else {
			  $results = $this -> fetch( $URI );
		  }

		  return $results;
	  }

	  function tabua_padrao() {
		  $html = $this -> get();


		  // Separa por dias
		  preg_match_all( $this -> regex_1, $html, $result, PREG_SET_ORDER );

		  foreach ( $result as $value ) {
			  $data = $value[ 1 ];

			  list($dia, $mes, $ano) = explode( '/', $data, 3 );

			  // Separa os horarios/alturas
			  preg_match_all( $this -> regex_2, $value[ 2 ], $result2, PREG_SET_ORDER );

			  foreach ( $result2 as $key2 => $value2 ) {
				  $dados[ $dia ][ $key2 ][ 'hora' ] = $value2[ 1 ];
				  $dados[ $dia ][ $key2 ][ 'altura' ] = $value2[ 2 ];
			  }
		  }

		  return $dados;
	  }

	  function ultimoDiaMes( $data = NULL ) {
		  if ( !$data ) {
			  $primeiro_dia = date( '01/m/Y' );
			  $dia = date( "d", $primeiro_dia );
			  $mes = date( "m", $primeiro_dia );
			  $ano = date( "Y", $primeiro_dia );
		  } else {
			  $dia = date( "d", $data );
			  $mes = date( "m", $data );
			  $ano = date( "Y", $data );
		  }
		  $data = mktime( 0, 0, 0, $mes, 1, $ano );
		  return date( "d", $data - 1 );
	  }

  }

